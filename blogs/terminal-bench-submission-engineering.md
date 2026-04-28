---
layout: page
permalink: /blogs/terminal-bench-submission-engineering/index.html
title: Terminal-Bench 2.0 提交复盘：从跑分、轨迹清理到 Hugging Face PR
description: 记录一次 Wecode GPT-5.5 提交 Terminal-Bench 2.0 leaderboard 的完整工程过程，包括跑分统计、95% 置信区间、metadata.yaml、trajectory、system prompt 清理、目录结构和 Hugging Face PR 提交。
---

<style>
.tbsub-lead{background:#f6fbfd;border-left:4px solid #4a7a8c;padding:1rem 1rem .9rem;border-radius:10px;margin:1rem 0 1.25rem}
.tbsub-note{color:#586674;font-size:.92rem;line-height:1.8}
.tbsub-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:1rem;margin:1rem 0 1.35rem}
.tbsub-card{background:#fbfcfe;border:1px solid #dce5ec;border-radius:10px;padding:1rem}
.tbsub-card h3{margin:.1rem 0 .55rem;color:#2f4756;font-size:1.02rem}
.tbsub-card p{margin:.35rem 0;color:#536572;line-height:1.75}
.tbsub-code{background:#fbfcfe;border:1px solid #dce5ec;border-radius:10px;padding:.95rem 1rem;margin:1rem 0 1.25rem}
.tbsub-code pre{margin:0;white-space:pre-wrap;word-break:break-word;font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;font-size:.9rem;line-height:1.65}
.tbsub-table{width:100%;border-collapse:collapse;margin:1rem 0 1.3rem;font-size:.95rem}
.tbsub-table th,.tbsub-table td{border-bottom:1px solid #dde4ea;padding:.72rem .55rem;text-align:left;vertical-align:top}
.tbsub-table th{color:#405361}
.tbsub-list li{margin:.42rem 0;line-height:1.75;color:#42586a}
@media (max-width: 800px){.tbsub-grid{grid-template-columns:1fr}}
</style>

## Terminal-Bench 2.0 提交复盘：从跑分、轨迹清理到 Hugging Face PR

> 更新时间：2026/04/28  
> 文章定位：一次真实 leaderboard submission 的工程复盘。  
> 提交结果：<a href="https://huggingface.co/datasets/harborframework/terminal-bench-2-leaderboard/discussions/164">Wecode GPT-5.5 PR #164</a>

<div class="tbsub-lead">
  <p>这篇不是 Terminal-Bench 2.0 的入门介绍。入门部分我已经单独写过一篇 <a href="/blogs/terminal-bench-2/">Terminal-Bench 2.0 完全导读</a>。这篇记录的是一次真实提交 leaderboard 的工程过程：怎么核算跑分，怎么理解误差范围，怎么处理 system prompt 和 trajectory，怎么补 metadata，怎么把本地 run 组织成 Hugging Face 官方接受的目录结构，最后怎么用 PR 提交。</p>
</div>

这次提交的对象是：

- Agent：Wecode
- Model：GPT-5.5
- Benchmark：Terminal-Bench 2.0
- Submission path：`submissions/terminal-bench/2.0/Wecode__GPT-5.5/`
- PR：<https://huggingface.co/datasets/harborframework/terminal-bench-2-leaderboard/discussions/164>

## 最终数据

这次本地 run 的核心统计是：

<table class="tbsub-table">
  <thead>
    <tr><th>项目</th><th>数值</th><th>说明</th></tr>
  </thead>
  <tbody>
    <tr><td>Tasks</td><td>89</td><td>Terminal-Bench 2.0 官方任务数</td></tr>
    <tr><td>Trials</td><td>445</td><td>每个 task 跑 5 次</td></tr>
    <tr><td>Passing trials</td><td>373</td><td>reward &gt; 0 的 trial</td></tr>
    <tr><td>Accuracy</td><td>83.8%</td><td>373 / 445 = 0.8382022472</td></tr>
    <tr><td>95% CI half-width</td><td>± 1.8</td><td>leaderboard 显示风格，单位是百分点</td></tr>
  </tbody>
</table>

本地校验结果：

<div class="tbsub-code">
<pre>metadata_valid True
job_dirs 1
trial_count 445
unique_tasks 89
min_trials_per_task 5
max_trials_per_task 5
successful_trials 373
missing_required_fields 0
passing_missing_trajectory 0
json_bad 0
reward_hacking_hits 0
errors 0</pre>
</div>

## 1. 跑分不是只看一个 mean

最容易先算的是 accuracy：

<div class="tbsub-code">
<pre>accuracy = passing_trials / total_trials
         = 373 / 445
         = 0.8382022472
         = 83.8%</pre>
</div>

但 leaderboard 上通常还会显示一个 `±` 误差范围。这里要特别小心：这个 `±` 不是方差，也不是标准差本身，而是 **95% confidence interval 的 half-width**，也就是置信区间半宽度。

这次我按 leaderboard 前端逻辑核对到的计算方式是：

<div class="tbsub-code">
<pre>stderr = sqrt(mean(per_task_sample_variance) / total_trials)
ci_half_width = 1.96 * stderr * 100</pre>
</div>

本次 run：

<div class="tbsub-code">
<pre>stderr = 0.0092654059
ci_half_width = 1.96 * 0.0092654059 * 100
              = 1.816
              ≈ ± 1.8</pre>
</div>

所以数学上它表达的是：如果把这次 benchmark 当作一次抽样估计，leaderboard 用这个半宽度表达 score 的不确定性。它不是“方差”；方差是平方量纲，不能直接拿来写成 `± 1.8%`。

## 2. metadata.yaml 要和真实提交对齐

官方 repo 的 README 要求每个 submission root 下必须有 `metadata.yaml`。我没有凭感觉写字段，而是对照了现有真实提交，例如：

- `Simple-Codex__GPT-5.3-Codex`
- `grok-cli__grok-4.20-0309-reasoning`
- `pilot-real__claude-opus-4-6`

最终使用的 metadata：

<div class="tbsub-code">
<pre>agent_url: https://pixboostai.com/
agent_display_name: "Wecode"
agent_org_display_name: "Gradence"

models:
  - model_name: gpt-5.5
    model_provider: openai
    model_display_name: "GPT-5.5"
    model_org_display_name: "OpenAI"</pre>
</div>

这里有一个坑：trajectory 里有一个内部字段类似 `model_provider: wecode`，但 leaderboard metadata 里的 `model_provider` 是模型提供商，不是 agent wrapper 的名字。所以这里应当填 `openai`，展示组织填 `OpenAI`。

## 3. 最大的目录坑：必须多包一层 job folder

我一开始把文件夹整理成了这样：

<div class="tbsub-code">
<pre>Wecode__GPT-5.5/
  metadata.yaml
  config.json
  result.json
  fix-code-vulnerability__qpPHMAd/
    result.json
    agent/
  gcode-to-text__P48eGu2/
  ...</pre>
</div>

看起来很合理，但不符合官方 importer 的结构。官方 importer 会把 submission root 下“带有 `config.json` 的目录”识别成 job directory。所以上面的扁平结构会让 445 个 trial 目录都被误识别成 job，然后每个 job 下面又找不到 trial，直接失败。

失败形态大概是：

<div class="tbsub-code">
<pre>official_job_dirs_detected 445
official_trial_count 0
official_unique_tasks 0
official_errors 446
jobs_with_no_nested_trials 445</pre>
</div>

正确结构是：

<div class="tbsub-code">
<pre>Wecode__GPT-5.5/
  metadata.yaml
  wecode-tb2-4-26-5/
    config.json
    result.json
    fix-code-vulnerability__qpPHMAd/
      result.json
      agent/
        trajectory.json
    gcode-to-text__P48eGu2/
    ...</pre>
</div>

整理后再跑校验，就变成：

<div class="tbsub-code">
<pre>job_dirs 1 ['wecode-tb2-4-26-5']
trial_count 445
unique_tasks 89
min_trials_per_task 5
max_trials_per_task 5
errors 0</pre>
</div>

工程经验：不要只看目录“人眼合理不合理”，要按 importer 的实际函数理解它怎么发现 job 和 trial。

## 4. trajectory 不能随便删

Terminal-Bench 的 leaderboard integrity update 强调了 trajectory 的重要性。尤其是 passing trials，trajectory 是审计 agent 行为的重要证据。

这次本地有：

<div class="tbsub-code">
<pre>trajectory_count 445
passing 373
passing_missing_trajectory 0</pre>
</div>

所以这里的原则是：

<ul class="tbsub-list">
  <li>不要为了“瘦身”删除 passing trial 的 `agent/trajectory.json`。</li>
  <li>如果要处理敏感内容，应做字段级清理，并保证 JSON 仍然可解析。</li>
  <li>清理后重新检查所有 passing trial 是否仍然有 trajectory。</li>
</ul>

这次 trajectory 是 ATIF-v1.6。清理 system prompt 时，我保留了 trajectory 文件本身，只移除 system/base prompt 内容。

## 5. system prompt 清理：别留 redaction 文案

这次我先扫描了所有 `agent/trajectory.json`，清理内容包括：

- 显式 `system_prompt` 字段
- `base_instructions_present: true`
- 嵌在 JSON 字符串里的 `system_prompt`
- 残留的 `AGENTS.md instructions` / `<INSTRUCTIONS>`

第一轮清理后，我曾经把残留字段替换成：

<div class="tbsub-code">
<pre>[REDACTED: base/system instructions removed]</pre>
</div>

后来发现这不是最干净的做法。最终改成空字符串 `""`，避免在提交内容中出现新的解释性文本。

最终扫描结果：

<div class="tbsub-code">
<pre>redaction_marker 0
system_prompt 0
AGENTS_INSTRUCTIONS 0
base_instr_true 0
json_bad 0
passing_missing_trajectory 0</pre>
</div>

这个步骤的 knowhow 是：清理敏感字段时，不能只 grep 一次。因为 system prompt 可能出现在结构化字段、嵌套对象、甚至 JSON-ish 字符串里。最好做结构化 JSON traversal，再做全文关键字扫描。

## 6. reward-hacking 字符串扫描

官方 importer 会扫描 `agent/*` 文件里是否出现某些敏感字符串，例如和 benchmark/repo/站点直接相关的内容。这类检测是为了防止 agent 在任务中访问 benchmark 网站、题库或公开答案。

本次本地扫描结果：

<div class="tbsub-code">
<pre>reward_hacking_hits 0</pre>
</div>

我的建议是提交前至少扫这几类：

<div class="tbsub-code">
<pre>harborframework
harbor-framework
laude-institute
tbench.ai</pre>
</div>

注意，这不是为了“绕过检测”，而是为了确认 agent 轨迹中没有违规访问 benchmark 资源的证据。

## 7. 上传前清理无关文件

macOS 会产生 `.DS_Store`。这类文件对 benchmark 没有任何价值，提交前应该删掉。

本次上传前发现两个：

<div class="tbsub-code">
<pre>Wecode__GPT-5.5/.DS_Store
Wecode__GPT-5.5/wecode-tb2-4-26-5/adaptive-rejection-sampler__d5SfNmE/.DS_Store</pre>
</div>

删除后再确认：

<div class="tbsub-code">
<pre>ds_store_files 0</pre>
</div>

这是小事，但这种小事会影响 reviewer 对提交质量的判断。

## 8. 用 PR 提交，而不是直接推 main

官方 README 写得很清楚：fork / branch / PR。使用 Hugging Face CLI 时，可以用 `--create-pr`：

<div class="tbsub-code">
<pre>hf upload harborframework/terminal-bench-2-leaderboard \
  /Users/jakefan/Wecode__GPT-5.5 \
  submissions/terminal-bench/2.0/Wecode__GPT-5.5 \
  --repo-type dataset \
  --create-pr \
  --commit-message "Add Wecode GPT-5.5 Terminal-Bench 2.0 submission"</pre>
</div>

本次生成的 PR：

<div class="tbsub-code">
<pre>https://huggingface.co/datasets/harborframework/terminal-bench-2-leaderboard/discussions/164</pre>
</div>

上传后我又做了远端抽查，确认这些关键文件都返回 HTTP 200：

<ul class="tbsub-list">
  <li><code>metadata.yaml</code></li>
  <li><code>wecode-tb2-4-26-5/config.json</code></li>
  <li><code>wecode-tb2-4-26-5/result.json</code></li>
  <li><code>wecode-tb2-4-26-5/fix-code-vulnerability__qpPHMAd/result.json</code></li>
  <li><code>wecode-tb2-4-26-5/fix-code-vulnerability__qpPHMAd/agent/trajectory.json</code></li>
</ul>

上传日志里出现了：

<div class="tbsub-code">
<pre>Skipped 446 file(s) in commit (ignored by gitignore file).</pre>
</div>

这类日志不能直接忽略。至少要抽查远端关键文件，确认 `result.json`、`config.json`、`trajectory.json` 这些提交必要文件没有被过滤掉。

## 9. 我会沉淀成固定 checklist 的部分

<div class="tbsub-grid">
  <div class="tbsub-card">
    <h3>跑分统计</h3>
    <p>统计 total trials、passing trials、unique task checksums、每 task trial 数、accuracy 和 CI half-width。不要把误差范围说成方差。</p>
  </div>
  <div class="tbsub-card">
    <h3>目录结构</h3>
    <p>submission root 下只放 metadata 和 job folder；trial 目录必须在 job folder 下面。</p>
  </div>
  <div class="tbsub-card">
    <h3>trajectory 审计</h3>
    <p>passing trial 不删 trajectory。清理敏感内容后必须重新验证 JSON 解析和 passing trajectory 覆盖。</p>
  </div>
  <div class="tbsub-card">
    <h3>远端验证</h3>
    <p>PR 创建成功不等于文件完整。用 raw URL 抽查关键文件，确认远端 commit 上真的存在。</p>
  </div>
</div>

## 10. 最后一点：benchmark 提交也是工程任务

这次最明显的感受是：benchmark submission 不是“把结果文件传上去”这么简单。

它更像一个小型 release：

- 数据要可复算
- 目录要符合 importer
- metadata 要和实际 agent / model 对齐
- trajectory 要保留审计价值
- 敏感字段要清理干净
- 远端 PR 要能被官方 bot 复现验证

如果没有这些工程动作，一个 83.8% 的结果也只是本地文件夹里的数字。只有当它能被官方 pipeline 接收、验证、审计，才算真正进入 leaderboard 的提交流程。

## 参考链接

<ul class="tbsub-list">
  <li>Terminal-Bench 2.0 leaderboard：<a href="https://www.tbench.ai/leaderboard/terminal-bench/2.0">https://www.tbench.ai/leaderboard/terminal-bench/2.0</a></li>
  <li>官方 submission repo：<a href="https://huggingface.co/datasets/harborframework/terminal-bench-2-leaderboard">https://huggingface.co/datasets/harborframework/terminal-bench-2-leaderboard</a></li>
  <li>官方 submission 目录：<a href="https://huggingface.co/datasets/harborframework/terminal-bench-2-leaderboard/tree/main/submissions/terminal-bench/2.0">submissions/terminal-bench/2.0</a></li>
  <li>本次 Wecode GPT-5.5 PR：<a href="https://huggingface.co/datasets/harborframework/terminal-bench-2-leaderboard/discussions/164">PR #164</a></li>
  <li>Leaderboard integrity update：<a href="https://www.tbench.ai/news/leaderboard-integrity-update">https://www.tbench.ai/news/leaderboard-integrity-update</a></li>
</ul>
