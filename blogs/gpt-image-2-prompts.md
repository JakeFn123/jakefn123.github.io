---
layout: page
permalink: /blogs/gpt-image-2-prompts/index.html
title: GPT-Image-2 提示词仓库整理：真正可复用的是四种写法
description: 基于 EvoLinkAI 的 awesome-gpt-image-2-prompts 仓库，整理出人像、海报、角色设定和 UI/信息图四类最值得复用的 prompt 结构，并补上可直接改写的中文案例模板。
---

## GPT-Image-2 提示词仓库整理：真正可复用的是四种写法

> 更新时间：2026/04/24  
> 文章定位：提示词结构整理与案例扩写。

最近看到一个整理得比较勤快的 GitHub 仓库：  
`EvoLinkAI/awesome-gpt-image-2-prompts`。

它的价值不在于“又多了几百条 prompt”，而在于它把社区里一段时间内比较典型的 GPT-Image-2 用法堆到了一起。你把这些 case 连起来看，会发现真正值得抄的不是句子，而是结构。

这个仓库把案例大致分成了几类：

- 人像与摄影
- 海报与插画
- 角色设计
- UI 与社交媒体截图
- 模型对比与社区实验

我把整份 `README_zh-CN.md` 重新过了一遍，决定不照着目录搬运，而是提炼成一篇更适合日常使用的笔记：  
**如果你想把 GPT-Image-2 用稳，优先学会四种 prompt 写法。**

---

<div class="blk-v2">
  <div class="sh-v2">来源说明</div>
</div>

<style>
.g2-note{color:#586674;font-size:.92rem;line-height:1.8}
.g2-callout{background:#f6fbfd;border-left:4px solid #4a7a8c;padding:1rem 1rem .9rem;border-radius:10px;margin:1rem 0}
.g2-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:1rem;margin:1rem 0 1.2rem}
.g2-card{background:#fbfcfe;border:1px solid #dce5ec;border-radius:10px;padding:1rem 1rem .9rem}
.g2-card h3{margin:.1rem 0 .45rem;font-size:1rem;line-height:1.45;color:#304552}
.g2-card p{margin:0;color:#556674;font-size:.92rem;line-height:1.72}
.g2-table{width:100%;border-collapse:collapse;margin:1rem 0 1.2rem;font-size:.96rem}
.g2-table th,.g2-table td{border-bottom:1px solid #dde4ea;padding:.75rem .55rem;text-align:left;vertical-align:top}
.g2-table th{color:#405361}
.g2-code{background:#fbfcfe;border:1px solid #dce5ec;border-radius:10px;padding:1rem 1rem .85rem;margin:1rem 0 1.2rem}
.g2-code pre{white-space:pre-wrap;font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;font-size:.9rem;line-height:1.68;margin:.2rem 0 0}
@media (max-width: 720px){
  .g2-grid{grid-template-columns:1fr}
}
</style>

<p class="g2-note">
源仓库：<a href="https://github.com/EvoLinkAI/awesome-gpt-image-2-prompts">EvoLinkAI/awesome-gpt-image-2-prompts</a><br>
主要参考文档：<a href="https://github.com/EvoLinkAI/awesome-gpt-image-2-prompts/blob/main/README_zh-CN.md">README_zh-CN.md</a><br>
仓库在 2026 年 4 月 23 日的更新说明里提到，已统一案例标题并持续补充 prompt 与案例图。下面的整理基于该版本 README。
</p>

---

<div class="blk-v2">
  <div class="sh-v2">先说结论：这个仓库真正值得学什么</div>
</div>

如果你只是想“找一条 prompt 复制进去试试”，这个仓库当然也有用。  
但如果你想形成自己的稳定写法，我觉得最该记住的是下面四类结构。

<div class="g2-grid">
  <div class="g2-card">
    <h3>1. 人像不是写“美女”就够了</h3>
    <p>高质量人像 prompt 的核心不是形容词堆叠，而是镜头、光线、肤质、姿态、服装和场景之间要互相咬合。</p>
  </div>
  <div class="g2-card">
    <h3>2. 海报不是画面描述，而是版式描述</h3>
    <p>好的海报类 prompt 往往会明确留白、主视觉路径、文字位置、比例和视觉重心，而不是只说“做一张很高级的海报”。</p>
  </div>
  <div class="g2-card">
    <h3>3. 角色设定图本质上是信息组织</h3>
    <p>角色类案例最稳定的做法，是把三视图、表情差分、装备拆解、色板和世界观说明拆开交代。</p>
  </div>
  <div class="g2-card">
    <h3>4. UI/信息图最吃结构约束</h3>
    <p>这类任务里，模型不是不会画，而是经常画得像“概念图”。想要更可用，就要把页面类型、模块组成和文字组织一起写清楚。</p>
  </div>
</div>

<div class="g2-callout">
  <p>我看完整个仓库后的最大感受是：很多 prompt 很长，但真正起决定作用的通常只有 5 到 7 个结构槽位。把槽位学会，比背原文有用得多。</p>
</div>

---

<div class="blk-v2">
  <div class="sh-v2">我整理出的 Prompt 骨架</div>
</div>

不管是人像、海报还是 UI，我最后都能压回这张表。

<table class="g2-table">
  <thead>
    <tr>
      <th>槽位</th>
      <th>它在解决什么</th>
      <th>常见写法</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>任务类型</td>
      <td>先告诉模型你要的不是“图片”，而是哪一类成品</td>
      <td><code>35mm portrait</code>、<code>city poster</code>、<code>character sheet</code>、<code>UI design system</code></td>
    </tr>
    <tr>
      <td>构图与比例</td>
      <td>决定画面组织方式</td>
      <td>特写、半身、鸟瞰、三视图、四屏拼接、<code>9:16</code>、<code>16:9</code>、<code>1:1</code></td>
    </tr>
    <tr>
      <td>主体描述</td>
      <td>定义谁是主角，以及主角有哪些关键视觉特征</td>
      <td>年龄感、五官、服装、材质、动作、姿态、道具</td>
    </tr>
    <tr>
      <td>环境与光线</td>
      <td>让风格从抽象词变成可渲染的物理场景</td>
      <td>便利店冷光、纸艺留白、窗边自然光、低饱和雾感、体积光</td>
    </tr>
    <tr>
      <td>版式与信息</td>
      <td>保证海报、设定卡、UI 不会只剩“漂亮”</td>
      <td>标题位置、竖排 slogan、色板区、参数区、模块列表</td>
    </tr>
    <tr>
      <td>约束条件</td>
      <td>压掉常见失真和跑偏</td>
      <td>不要塑料皮肤、不要水印、文字清晰、布局整齐、背景不要过满</td>
    </tr>
  </tbody>
</table>

这也是为什么很多新手 prompt 不稳定。  
不是词不够华丽，而是经常只写了“风格”和“主体”，没写构图、版式和约束。

---

<div class="blk-v2">
  <div class="sh-v2">四类最值得抄的写法</div>
</div>

### 1. 人像类：把“镜头语言”写出来

仓库里的人像类 case 很多，最稳定的一批并不是词最夸张的，而是摄影信息最完整的。比如：

- `35mm film`
- `soft diffused natural window light`
- `direct on-camera flash`
- `medium shot`
- `soft film grain`
- `slight overexposure`

这类词的价值在于，它们会把“审美风格”落到摄影条件上。  
你写“高级感”“电影感”很空，但你写“35mm、窗边柔光、轻微过曝、低对比、近身中景”，模型更容易收敛。

我更推荐把人像 prompt 写成这样：

<div class="g2-code">
  <strong>可复用模板：日常电影感人像</strong>
  <pre>35mm film photography, soft natural window light, slight overexposure, low contrast, subtle film grain,
eye-level medium shot, young East Asian woman, natural skin texture, minimal makeup,
oversized white shirt, relaxed standing pose near a window, clean indoor background with large negative space,
quiet everyday mood, realistic fabric wrinkles, natural hair strands,
no plastic skin, no watermark, no text, aspect ratio 9:16</pre>
</div>

如果要继续提高命中率，我建议再补两层：

- 先写拍摄条件：镜头、光线、角度、景别
- 再写人和衣服：年龄感、妆发、材质、动作
- 最后写氛围和禁区：安静、克制、不要过饱和、不要塑料皮

### 2. 海报类：先想视觉路径，再写画面元素

海报类是这个仓库里我觉得最值得看的部分。原因很简单：  
它不是“描一张图”，而是在训练模型做信息设计。

像 `Boston Spring 2026 City Poster` 这类案例，真正厉害的地方不只是元素多，而是它写清楚了：

- 背景是干净留白
- 主视觉是一条会变形的河流
- 城市元素被装进河流路径里
- 文字在左下角
- 画面比例是 `9:16`

也就是说，它交代的是版式逻辑，不只是题材。

<div class="g2-code">
  <strong>可复用模板：城市主题海报</strong>
  <pre>Create a vertical city poster with a refined editorial layout.
Off-white textured background with large negative space.
In the lower corner, a tiny human figure creates a flowing visual trail that transforms into the city's river and skyline.
Inside the flowing shape, include iconic landmarks, local architecture, and seasonal atmosphere.
Elegant typography placed in the lower left: main title, year, and one vertical slogan.
Fresh but restrained colors, layered depth, premium poster composition, text clear and complete, aspect ratio 9:16.</pre>
</div>

这类 prompt 的关键不在于“城市元素列多少个”，而在于：

- 主视觉路径是什么
- 信息区放哪
- 留白留多少
- 元素是铺满，还是被装进一个形状里

如果这四件事不写，最后很容易变成“元素堆砌图”。

### 3. 角色设定类：把角色图当成文档，不要当成插画

角色设定卡这一类，仓库里最稳的 prompt 都有一个共性：  
**它们在要求模型画一份资料页，而不是画一张帅图。**

像 `Persona5 Character Reference Card` 和日文版的 `Official Character Sheet`，结构都非常像：

- 正面、侧面、背面
- 表情变化
- 服装与装备拆解
- 色板
- 世界观一句话
- 白底、图解式布局

这类任务其实特别适合做二创、游戏前设、视觉开发。

<div class="g2-code">
  <strong>可复用模板：角色设定资料卡</strong>
  <pre>Based on this character, create an official-style character sheet.
Include front, side, and back views.
Add facial expression variations.
Break down costume and equipment parts with small callouts.
Add a color palette and a short worldbuilding note.
Use a clean white background and organized editorial layout.
High-resolution professional concept art style, aspect ratio 16:9.</pre>
</div>

如果你做原创角色，我建议把 prompt 再补完整一点：

- 角色定位：学生、赏金猎人、神官、机甲维修师
- 视觉锚点：一件最有辨识度的服装或武器
- 设定页模块：三视图、差分、装备、色板、设定文字

这样出来的图才更像“可继续开发的素材”，而不是一次性好看的封面。

### 4. UI 与信息图类：明确它是“页面”，不是“氛围板”

很多人让模型做 UI，会得到一张很美但很难继续用的图。  
仓库里 `One-Prompt UI Design Generation` 这种 case 虽然提示词很短，但它至少点明了一个方向：

- 不只要单个界面
- 而是要一整套系统
- 包含网页、移动端、卡片、控件、按钮

这个思路是对的。  
真正要提高稳定性，只需要把模块再写具体一点。

<div class="g2-code">
  <strong>可复用模板：风格参考图到 UI 系统</strong>
  <pre>Use this reference style to generate a complete UI design system.
Include a web landing page, mobile screen, dashboard card variants, buttons, input fields, modal, chart card, and navigation patterns.
Keep the visual language consistent across all components.
Show multiple surfaces in one organized presentation board.
Clean typography, clear spacing system, consistent colors, practical interface layout, high-end product design presentation.</pre>
</div>

如果你要的是信息图，而不是 UI，本质上也一样。  
你得把它从“做得酷一点”改成“信息如何被分块展示”。

比如仓库里一些百科卡、关系图、流程图类 case，本质上都在做三件事：

- 定主题
- 定版式模块
- 定每个模块里放什么

---

<div class="blk-v2">
  <div class="sh-v2">五个我觉得最有用的案例</div>
</div>

下面这五个例子，不是仓库里“最好看”的全部，但我觉得是最有代表性的。

<table class="g2-table">
  <thead>
    <tr>
      <th>案例</th>
      <th>我觉得它值得看什么</th>
      <th>对应能力</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>Soft Airy 35mm Portrait</code></td>
      <td>把光线、景别、材质和氛围写得足够物理化</td>
      <td>摄影感人像</td>
    </tr>
    <tr>
      <td><code>Boston Spring 2026 City Poster</code></td>
      <td>把城市元素装进一条主视觉路径里，而不是散点罗列</td>
      <td>海报构图与排版</td>
    </tr>
    <tr>
      <td><code>Persona5 Character Reference Card</code></td>
      <td>把“设定页”拆成模块，适合角色开发</td>
      <td>角色资料卡</td>
    </tr>
    <tr>
      <td><code>One-Prompt UI Design Generation</code></td>
      <td>提醒你 UI 不该只生成单屏，而要生成系统</td>
      <td>界面系统表达</td>
    </tr>
    <tr>
      <td><code>Wooden Bookshelf Prompt Test</code></td>
      <td>它很朴素，但特别适合测试模型的计数、空间和约束理解</td>
      <td>结构遵循能力</td>
    </tr>
  </tbody>
</table>

最后这个书架案例其实很重要。  
很多时候你不是要测“美不美”，而是要测模型是否真的理解你的约束。

像这种 prompt：

<div class="g2-code">
  <strong>结构测试模板：计数与空间约束</strong>
  <pre>A wooden bookshelf with three shelves:
top shelf has one book,
middle shelf has three books,
bottom shelf has seven books.
Front view, clean composition, objects clearly separated, realistic lighting.</pre>
</div>

它的价值在于，你可以很快知道模型有没有把“局部规则”稳定落图。  
做商品图、信息图、卡片布局时，这类测试比审美词更实用。

---

<div class="blk-v2">
  <div class="sh-v2">如果你只想记一套中文写法</div>
</div>

如果你平时主要是中文工作流，我建议直接记住这套顺序：

<div class="g2-code">
  <strong>通用中文骨架</strong>
  <pre>请生成一张【成品类型】。
画面比例为【比例】。
主体是【人物/物体/场景】，
采用【镜头/视角/景别】，
光线为【自然光/闪光灯/体积光/霓虹混光】，
整体风格为【摄影/海报/概念设定/UI 展示板】，
背景与环境包含【关键场景元素】，
版式中需要明确包含【标题/色板/模块/按钮/说明文字】，
材质与细节强调【肤质/布料/金属/纸张/玻璃】，
整体氛围是【克制/安静/高饱和/未来感/复古感】，
并避免【水印/文字错误/塑料质感/过度拥挤/低清晰度】。</pre>
</div>

这套写法看起来很普通，但它有一个好处：  
你不会漏掉最关键的结构槽位。

---

<div class="blk-v2">
  <div class="sh-v2">我的结论</div>
</div>

这份 `awesome-gpt-image-2-prompts` 仓库当然适合收藏，但如果只是把它当“prompt 素材库”，其实有点浪费。

我更建议把它当成一个观察样本库去看：

- 人像类学镜头和光线
- 海报类学主视觉路径和留白
- 角色类学信息分层
- UI 类学模块化表达
- 对比类拿来做约束测试

真正能迁移到你自己工作流里的，不是某一条神奇 prompt，  
而是你能不能把任务稳定拆成：**类型、构图、主体、环境、版式、约束**。

这六件事写清楚了，GPT-Image-2 的出图稳定性会比单纯堆形容词高很多。

---

<div class="blk-v2">
  <div class="sh-v2">来源注记</div>
</div>

<p class="g2-note">
本文基于 EvoLinkAI 开源仓库 <code>awesome-gpt-image-2-prompts</code> 的中文 README 整理而成。文中对案例的归类、优先级判断和模板化改写，属于我的二次整理与扩写，不等同于源仓库原文结构。<br>
原始来源：<a href="https://github.com/EvoLinkAI/awesome-gpt-image-2-prompts">https://github.com/EvoLinkAI/awesome-gpt-image-2-prompts</a><br>
中文文档：<a href="https://github.com/EvoLinkAI/awesome-gpt-image-2-prompts/blob/main/README_zh-CN.md">README_zh-CN.md</a>
</p>
