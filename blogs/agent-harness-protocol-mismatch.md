---
layout: page
permalink: /blogs/agent-harness-protocol-mismatch/index.html
title: LLM Agent Harness 的协议适配问题：从一次 GLM-5.1 接入失败说起
description: 记录一次把硅基流动 Pro/zai-org/GLM-5.1 接入 Codex/Wecode 类 agent harness 的真实排障过程，并分析 Responses API 与 Chat Completions API 在 agent 系统里的协议边界。
---

<style>
.proto-lead{background:#f6fbfd;border-left:4px solid #4a7a8c;padding:1rem 1rem .9rem;border-radius:10px;margin:1rem 0 1.25rem}
.proto-note{color:#586674;font-size:.92rem;line-height:1.8}
.proto-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:1rem;margin:1rem 0 1.35rem}
.proto-card{background:#fbfcfe;border:1px solid #dce5ec;border-radius:10px;padding:1rem}
.proto-card h3{margin:.1rem 0 .55rem;color:#2f4756;font-size:1.02rem}
.proto-card p{margin:.35rem 0;color:#536572;line-height:1.75}
.proto-code{background:#fbfcfe;border:1px solid #dce5ec;border-radius:10px;padding:.95rem 1rem;margin:1rem 0 1.25rem}
.proto-code pre{margin:0;white-space:pre-wrap;word-break:break-word;font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;font-size:.9rem;line-height:1.65}
.proto-table{width:100%;border-collapse:collapse;margin:1rem 0 1.3rem;font-size:.95rem}
.proto-table th,.proto-table td{border-bottom:1px solid #dde4ea;padding:.72rem .55rem;text-align:left;vertical-align:top}
.proto-table th{color:#405361}
.proto-list li{margin:.42rem 0;line-height:1.75;color:#42586a}
.proto-flow{display:grid;grid-template-columns:1fr auto 1fr auto 1fr;gap:.6rem;align-items:center;margin:1rem 0 1.3rem}
.proto-node{background:#fbfcfe;border:1px solid #dce5ec;border-radius:10px;padding:.85rem;text-align:center;color:#405361;line-height:1.55}
.proto-arrow{color:#4a7a8c;font-weight:700}
@media (max-width: 800px){.proto-grid{grid-template-columns:1fr}.proto-flow{grid-template-columns:1fr}.proto-arrow{text-align:center;transform:rotate(90deg)}}
</style>

## LLM Agent Harness 的协议适配问题：从一次 GLM-5.1 接入失败说起

> 更新时间：2026/05/02  
> 文章定位：一次真实 agent harness 接入排障的技术复盘。  
> 关键词：Agent harness, OpenAI Responses API, Chat Completions, SiliconFlow, Wecode, protocol adapter

<div class="proto-lead">
  <p>这篇文章记录一个很小但很有代表性的工程问题：我尝试把硅基流动上的 <code>Pro/zai-org/GLM-5.1</code> 接到自己的类 Codex/Wecode agent harness，最初以为只是 <code>config.toml</code> 和 <code>auth.json</code> 没写对，最后发现根因并不在配置，而在模型服务和 agent harness 之间的 wire protocol 不兼容。</p>
</div>

这个问题值得单独写下来，是因为它揭示了一个容易被忽略的事实：**模型 API 能聊，不等于它能被 agent harness 使用**。Coding agent 不是普通聊天客户端。它依赖流式事件、工具调用、状态标识、错误恢复和上下文压缩策略。只要这些协议层不匹配，模型本身再强也接不进去。

## 1. 症状：一个看起来像配置错误的 404

最初的目标很直接：在远程机器的 `~/.wecode/config.toml` 里增加一个硅基流动 provider，让 Wecode 使用 GLM-5.1：

<div class="proto-code">
<pre>model_provider = "siliconflow"
model = "Pro/zai-org/GLM-5.1"
disable_response_storage = true

[model_providers.siliconflow]
name = "SiliconFlow CN"
base_url = "https://api.siliconflow.cn/v1"
wire_api = "responses"
requires_openai_auth = true</pre>
</div>

`auth.json` 的思路也很自然：把硅基流动 API key 放到 OpenAI-compatible harness 常见的 `OPENAI_API_KEY` 字段中。

但启动后直接报错：

<div class="proto-code">
<pre>Unexpected status 404 Not Found: Not Found,
url: https://api.siliconflow.cn/v1/responses</pre>
</div>

第一眼看上去，这很像下面几类常见问题：

<ul class="proto-list">
  <li>API key 没有写对，或者 <code>auth.json</code> 的字段名不对。</li>
  <li>模型名写错，例如 <code>Pro/zai-org/GLM-5.1</code> 和 <code>zai-org/GLM-5.1</code> 混淆。</li>
  <li>域名选错，例如 <code>api.siliconflow.cn</code> 和 <code>api.siliconflow.com</code>。</li>
  <li>provider 的 <code>base_url</code> 多写或少写了 <code>/v1</code>。</li>
</ul>

这些都值得检查，但这次都不是根因。真正有用的线索在错误 URL 本身：程序请求的是 `/v1/responses`。

## 2. 快速定位：服务端没有这个 endpoint

硅基流动的 OpenAI-compatible 接口主要是 Chat Completions 风格，也就是：

<div class="proto-code">
<pre>POST https://api.siliconflow.cn/v1/chat/completions</pre>
</div>

可以用一个最小 curl 验证服务本身是否可用：

<div class="proto-code">
<pre>curl https://api.siliconflow.cn/v1/chat/completions \
  -H "Authorization: Bearer $SILICONFLOW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Pro/zai-org/GLM-5.1",
    "messages": [{"role": "user", "content": "hello"}],
    "stream": false
  }'</pre>
</div>

这一步的意义是把问题分层。如果 `/chat/completions` 成功，而 harness 请求 `/responses` 失败，那么问题就不是 key，也不是模型不可用，而是客户端协议选错了。

## 3. 第二个错误：`wire_api = "chat"` 已经被移除

直觉上的修复是把配置改成：

<div class="proto-code">
<pre>wire_api = "chat"</pre>
</div>

但新版 Codex/Wecode 类 harness 会直接拒绝这个配置：

<div class="proto-code">
<pre>Error loading config.toml:
/home/fcy/.wecode/config.toml:11:12: `wire_api = "chat"` is no longer supported.
How to fix: set `wire_api = "responses"` in your provider config.
More info: https://github.com/openai/codex/discussions/7782
   |
11 | wire_api = "chat"</pre>
</div>

这条错误把问题彻底钉住了：

<table class="proto-table">
  <thead>
    <tr><th>组件</th><th>支持的协议</th><th>实际 endpoint</th><th>结论</th></tr>
  </thead>
  <tbody>
    <tr><td>新版 Codex/Wecode harness</td><td>Responses API</td><td><code>/v1/responses</code></td><td>只会按 Responses 协议发请求</td></tr>
    <tr><td>SiliconFlow GLM-5.1</td><td>Chat Completions API</td><td><code>/v1/chat/completions</code></td><td>不提供 <code>/v1/responses</code></td></tr>
  </tbody>
</table>

所以这是一个协议不兼容问题。`config.toml` 可以选择 provider，但不能让服务商凭空支持一个不存在的 HTTP endpoint。

## 4. 为什么这不是“改个 URL”就能解决

很多模型 API 都自称 OpenAI-compatible。但在 agent harness 里，OpenAI-compatible 这句话必须拆开问：兼容的是哪一个 API？

<div class="proto-grid">
  <div class="proto-card">
    <h3>Chat Completions</h3>
    <p>经典聊天接口。核心输入是 <code>messages</code>，输出是 assistant message 或 streaming delta。很多第三方模型服务兼容的是这一层。</p>
  </div>
  <div class="proto-card">
    <h3>Responses API</h3>
    <p>更适合 agent 的统一接口。除了文本，还要承载 response id、工具调用、推理内容、结构化 output item 和更细粒度的事件流。</p>
  </div>
</div>

普通聊天客户端可能只需要：

<div class="proto-code">
<pre>messages -> assistant text</pre>
</div>

但 coding agent harness 通常需要：

<div class="proto-code">
<pre>conversation state
  -> model request
  -> streaming output events
  -> tool call events
  -> shell/apply_patch execution
  -> tool result injection
  -> model continuation
  -> final answer
  -> trace and persistence</pre>
</div>

这意味着协议层至少包含：

<ul class="proto-list">
  <li><strong>消息结构</strong>：Responses 的 input item 与 Chat Completions 的 role messages 不完全同构。</li>
  <li><strong>流式事件</strong>：Chat 的 delta chunk 需要翻译成 harness 期待的 response events。</li>
  <li><strong>工具调用</strong>：tool call 的 id、name、arguments、增量拼接和完成事件都要保持一致。</li>
  <li><strong>状态管理</strong>：Responses API 常带有 response id、previous response、output item 等状态语义。</li>
  <li><strong>错误归一化</strong>：供应商错误码、限流、模型不存在、鉴权失败都要转成 harness 能理解的形态。</li>
  <li><strong>审计与压缩</strong>：长程任务依赖 trace、rollout、上下文压缩摘要，协议事件如果丢失，后续排障会很困难。</li>
</ul>

因此，简单把 `/responses` 字符串替换成 `/chat/completions` 往往只能让第一跳 HTTP 成功，不能保证 agent 真的能完成任务。

## 5. 一个更准确的心理模型：模型不是后端，协议适配器才是后端

在 agent 系统里，模型服务最好不要被当成“直接可替换后端”。更稳的抽象是：

<div class="proto-flow">
  <div class="proto-node">Agent Harness<br><code>/v1/responses</code></div>
  <div class="proto-arrow">-></div>
  <div class="proto-node">Protocol Adapter<br>schema + stream + tools</div>
  <div class="proto-arrow">-></div>
  <div class="proto-node">Model Provider<br><code>/v1/chat/completions</code></div>
</div>

这层 adapter 不是网关转发器，而是语义翻译器。它需要知道 harness 的事件模型，也要知道供应商的能力边界。

如果没有 adapter，那么 `base_url` 的配置只能覆盖网络地址，不能覆盖协议语义。今天这个 404 就是最直接的例子：网络到了，协议没到。

## 6. 三种解决方案

这次问题的实际可选方案并不多：

<table class="proto-table">
  <thead>
    <tr><th>方案</th><th>工程成本</th><th>稳定性</th><th>适合场景</th></tr>
  </thead>
  <tbody>
    <tr><td>换支持 Responses API 的供应商或网关</td><td>低</td><td>高</td><td>想快速让新版 Codex/Wecode 跑起来</td></tr>
    <tr><td>换支持 Chat Completions 的旧版 harness</td><td>中</td><td>中</td><td>明确想继续用硅基流动模型，且可以接受旧协议限制</td></tr>
    <tr><td>写 Responses-to-Chat adapter</td><td>高</td><td>取决于实现</td><td>正在构建自己的 agent platform，需要统一接多家模型服务</td></tr>
  </tbody>
</table>

如果只是个人临时使用，最省事的是前两种。如果目标是长期维护自己的 agent harness，第三种才是正确的工程方向。

## 7. 适配层应该怎么设计

一个最小可用 adapter 可以从非流式文本开始，但真正用于 coding agent，至少要逐步覆盖下面这些模块：

<div class="proto-grid">
  <div class="proto-card">
    <h3>Request Translator</h3>
    <p>把 Responses request 中的 input、instructions、tools 和模型参数转换成 Chat Completions 的 messages、tools、tool_choice 和 stream 参数。</p>
  </div>
  <div class="proto-card">
    <h3>Stream Translator</h3>
    <p>把 Chat Completions 的 SSE chunks 转成 harness 期待的 response events，尤其是文本增量、工具参数增量和 completed 事件。</p>
  </div>
  <div class="proto-card">
    <h3>Tool Semantics</h3>
    <p>确保 tool call id 稳定，arguments 可以增量拼接，工具结果能回灌给下一轮模型请求，而不是只在文本里描述。</p>
  </div>
  <div class="proto-card">
    <h3>Failure Contract</h3>
    <p>把 401、404、429、5xx、模型不存在、上下文过长等错误归一成 harness 可诊断的错误，避免只暴露低信息量 HTTP 异常。</p>
  </div>
</div>

更完整的 adapter 还要处理模型能力声明：

<ul class="proto-list">
  <li>是否支持 tool calling。</li>
  <li>是否支持 parallel tool calls。</li>
  <li>是否支持 reasoning content。</li>
  <li>最大上下文长度和输出长度。</li>
  <li>streaming 下 tool arguments 是否稳定。</li>
  <li>供应商对 system/developer/user 角色的支持差异。</li>
</ul>

这些能力不应该散落在 prompt 里，而应该成为 provider capability metadata。否则 agent 在某个模型上能跑，在另一个模型上随机失败，最后很难定位。

## 8. 配置文件应该更早失败

这次排障里最浪费时间的地方，是错误看起来像鉴权或模型名问题。一个更好的 harness 应该在启动时做 provider preflight：

<div class="proto-code">
<pre>1. 读取 model_provider 和 wire_api
2. 检查 binary 是否支持这个 wire_api
3. 检查 provider 是否声明支持目标 endpoint
4. 用最小请求验证 endpoint 和模型名
5. 输出协议层诊断，而不是等到任务中途 404</pre>
</div>

理想错误信息应该像这样：

<div class="proto-code">
<pre>Provider protocol mismatch:
  harness wire_api: responses
  requested endpoint: /v1/responses
  provider appears to support: /v1/chat/completions

Config-only fix is not available. Use a Chat-compatible harness,
a Responses-compatible provider, or a protocol adapter.</pre>
</div>

这比裸露的 `Unexpected status 404 Not Found` 更能帮助用户做正确决策。

## 9. 从这次问题里得到的结论

<div class="proto-lead">
  <p>这次真正的修复不是某一行配置，而是把问题从“怎么写 config.toml”重新表述成“agent harness 与模型服务之间的协议契约是否一致”。</p>
</div>

我的几个判断：

<ul class="proto-list">
  <li><strong>模型兼容性不是 API key 兼容性。</strong>API key 能通过鉴权，只说明网络和账户层没问题，不说明 agent 协议可用。</li>
  <li><strong>OpenAI-compatible 不是一个单一标准。</strong>很多服务兼容的是 Chat Completions，不是 Responses API。</li>
  <li><strong>Agent harness 应该把 protocol adapter 作为一等组件。</strong>模型路由不只是选 base URL，还要选择 schema、stream、tool 和 failure contract。</li>
  <li><strong>配置文件不能弥补协议缺失。</strong>当 binary 移除了 <code>wire_api = "chat"</code>，继续调 config 只会绕圈。</li>
  <li><strong>长程 agent 任务更需要协议清晰。</strong>一次聊天可以容忍轻微格式差异，但 coding agent 的工具调用和上下文状态会把协议差异放大成系统性失败。</li>
</ul>

## 10. 后续如果要继续接 GLM-5.1

如果我后面继续把 GLM-5.1 接到自己的 agent harness，我会走 adapter 路线，而不是继续试图用新版 Codex 的 `responses` provider 直连硅基流动。

优先级大概是：

<div class="proto-code">
<pre>Phase 1: non-streaming text completion
Phase 2: streaming text delta
Phase 3: single tool call
Phase 4: incremental tool arguments
Phase 5: multi-turn tool result injection
Phase 6: failure normalization and trace persistence
Phase 7: benchmark against existing Responses-native provider</pre>
</div>

只有 Phase 5 之后，它才算是一个真正能跑 coding agent 的适配层。Phase 1 的“能回复 hello”只能证明模型 API 可用，不能证明 agent harness 可用。

## 参考

<ul class="proto-list">
  <li>SiliconFlow Chat Completions API：<a href="https://docs.siliconflow.cn/cn/api-reference/chat-completions/chat-completions">https://docs.siliconflow.cn/cn/api-reference/chat-completions/chat-completions</a></li>
  <li>OpenAI Codex discussion: <code>wire_api = "chat"</code> no longer supported：<a href="https://github.com/openai/codex/discussions/7782">https://github.com/openai/codex/discussions/7782</a></li>
  <li>本文基于 2026/05/02 一次真实 Wecode/Codex-like harness 排障记录整理。具体供应商接口和 Codex 行为可能随版本变化，需要以当时文档和 binary 行为为准。</li>
</ul>
