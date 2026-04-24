---
layout: page
permalink: /blogs/gpt-image-2-prompts/index.html
title: GPT-Image-2 提示词精选：我从这个 GitHub 仓库里挑了 10 个最值得抄的案例
description: 基于 EvoLinkAI 的 awesome-gpt-image-2-prompts 仓库，按人像、海报、角色设定、UI 和约束测试五类整理 10 个最值得复用的案例，保留原 prompt、原结果图和原始链接。
---

<style>
.gpt2-note{color:#586674;font-size:.92rem;line-height:1.8}
.gpt2-lead{background:#f6fbfd;border-left:4px solid #4a7a8c;padding:1rem 1rem .9rem;border-radius:10px;margin:1rem 0 1.2rem}
.gpt2-grid{display:grid;grid-template-columns:1.05fr .95fr;gap:1rem;align-items:start;margin:1rem 0 1.4rem}
.gpt2-card{background:#fbfcfe;border:1px solid #dce5ec;border-radius:12px;padding:1rem}
.gpt2-card h3{margin:.15rem 0 .6rem;color:#2f4756}
.gpt2-card p{margin:.4rem 0;color:#536572;line-height:1.75}
.gpt2-card img{width:100%;height:auto;display:block;border-radius:10px;box-shadow:0 8px 24px rgba(0,0,0,.08)}
.gpt2-meta{font-size:.9rem;color:#60707d;line-height:1.7}
.gpt2-prompt{background:#fbfcfe;border:1px solid #dce5ec;border-radius:12px;padding:1rem;margin:1rem 0 1.4rem}
.gpt2-prompt pre{white-space:pre-wrap;word-break:break-word;margin:0;font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;font-size:.9rem;line-height:1.7}
.gpt2-list{margin:0;padding-left:1.1rem}
.gpt2-list li{margin:.38rem 0;color:#536572;line-height:1.7}
.gpt2-divider{height:1px;background:#dde4ea;margin:1.6rem 0}
@media (max-width: 900px){
  .gpt2-grid{grid-template-columns:1fr}
}
</style>

## GPT-Image-2 提示词精选：我从这个 GitHub 仓库里挑了 10 个最值得抄的案例

> 更新时间：2026/04/24  
> 这次不是镜像整份 README，而是做一篇真正可读的精选版。

<div class="gpt2-lead">
  <p>这篇重做后的目标很简单：保留原 prompt、原结果图、原始来源链接，但不再把 3000 多行仓库内容整页硬贴进博客。相反，我从 <code>EvoLinkAI/awesome-gpt-image-2-prompts</code> 里挑了 10 个最能代表 GPT-Image-2 能力边界的案例，按五类整理成一篇能读、能抄、能回看的文章。</p>
</div>

<p class="gpt2-note">
原始仓库：<a href="https://github.com/EvoLinkAI/awesome-gpt-image-2-prompts">EvoLinkAI/awesome-gpt-image-2-prompts</a><br>
参考文档：<a href="https://github.com/EvoLinkAI/awesome-gpt-image-2-prompts/blob/main/README_zh-CN.md">README_zh-CN.md</a>
</p>

## 我为什么只挑 10 个

完整仓库当然适合收藏，但直接放进博客页会有两个问题：

- 信息量太大，读者很难知道先看什么
- 图、prompt、分类全堆在一起，不利于形成自己的复用模板

所以我这次只保留五类最有代表性的能力：

1. 人像摄影
2. 海报设计
3. 角色设定
4. UI / 社交媒体截图
5. 约束遵循与细节测试

---

## 1. 人像摄影

### Case 1: Soft Airy 35mm Portrait

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>这类人像 prompt 的价值不在“大词很多”，而在于它把镜头语言写得很具体：胶片感、窗边柔光、轻微过曝、低对比、空气感、安静日常氛围。它是那种很适合被改成自己日常模特、穿搭、室内摄影模板的 case。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/BubbleBrain/status/2046115431144902732">Soft Airy 35mm Portrait</a><br>
      作者：<a href="https://x.com/BubbleBrain">@BubbleBrain</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/portrait_case6/output.jpg" alt="Soft Airy 35mm Portrait">
  </div>
</div>

<div class="gpt2-prompt">
<pre>Analog 35mm film photography, soft airy Japanese-style aesthetic, gentle diffused natural window light, slight overexposure, pastel tones, low contrast, soft highlights, minimal indoor setting near a window with white curtains, clean light-colored wall, natural composition, eye-level, slightly closer full-body framing (mid-thigh to head), young East Asian woman, natural minimal makeup, soft realistic skin texture, long slightly messy dark hair, oversized white button-up shirt, light casual shorts, barefoot, simple and relaxed styling, standing naturally with relaxed posture, arms loosely at sides or slightly behind, facing camera, gentle soft smile, subtle stillness, focus on light, air, and quiet everyday mood, soft film grain, dreamy and understated atmosphere --ar 9:16</pre>
</div>

### Case 2: Convenience Store Neon Portrait

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>这是另一种极端：不是轻空气感，而是便利店冷光加霓虹混光。它说明 GPT-Image-2 在“摄影条件 + 人物细节 + 场景反射”同时存在时，也能维持相当强的画面统一性。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/BubbleBrain/status/2045167461147042202">Convenience Store Neon Portrait</a><br>
      作者：<a href="https://x.com/BubbleBrain">@BubbleBrain</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/portrait_case1/output.jpg" alt="Convenience Store Neon Portrait">
  </div>
</div>

<div class="gpt2-prompt">
<pre>35mm film photography with harsh convenience store fluorescent lighting mixed with colorful neon signs from outside, authentic film grain, high contrast, slight color cast, cinematic street editorial style, intimate medium shot, early 20s sexy Chinese female idol with ultra-realistic delicate refined Chinese features, seductive almond-shaped fox eyes with natural double eyelids, high nose bridge, small sharp V-shaped jawline, flawless porcelain skin with cool ivory undertone and visible specular highlights from fluorescent light, subtle skin texture and micro pores, natural dewy makeup with soft flush on cheeks, glossy natural pink lips slightly parted, subtle natural freckles across nose and cheeks, long dark brown hair in a messy high ponytail with many loose strands falling around face and neck, wearing an oversized white button-up shirt as the only top, unbuttoned at the top with deep cleavage and loosely tied at the waist, paired with a tiny black pleated mini skirt, barefoot in simple white slides, seductive casual leaning pose against the glass door of a 24-hour convenience store at late night, body slightly arched, one leg bent with foot resting against the door frame, the other leg straight, one hand holding a bottle of iced drink, the other hand lightly pulling the hem of her mini skirt, intensely seductive playful yet slightly vulnerable gaze straight at the viewer with soft doe eyes full of quiet temptation and teasing smile, bright cold fluorescent store light from inside mixed with pink and blue neon glow from outside signs, realistic reflections on glass door, blurred convenience store interior with shelves and snacks in background, authentic 35mm film color grading with harsh lighting and neon accents, extremely sharp yet soft skin rendering, natural hair strands, realistic fabric wrinkles and drape on the oversized shirt and mini skirt, no plastic skin, no digital over-sharpening, no airbrushing, no blemishes, no moles, no oily skin, no watermark, no text, authentic late-night convenience store atmosphere</pre>
</div>

---

## 2. 海报设计

### Case 3: Boston Spring 2026 City Poster

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>这不是“把波士顿元素都画出来”，而是先定义一条视觉路径，再把城市意象装进这条路径里。也就是它做的是版式，不是简单插画。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/BubbleBrain/status/2045358053831172358">Boston Spring 2026 City Poster</a><br>
      作者：<a href="https://x.com/BubbleBrain">@BubbleBrain</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/poster_case1/output.jpg" alt="Boston Spring 2026 City Poster">
  </div>
</div>

<div class="gpt2-prompt">
<pre>A striking Spring 2026 city poster for Boston with an elegant celebratory mood and a bold contemporary design. On a clean off-white textured background with large areas of negative space, a miniature single sculler rows across the lower right corner of the image on a narrow ribbon of reflective water. The wake from the oar sweeps upward in a dynamic calligraphic curve, gradually transforming into the Charles River and then into a dreamlike hand-painted panorama of Boston. Inside this flowing river-shaped composition are iconic Boston elements: the Back Bay skyline, Beacon Hill brownstones, Acorn Street, Boston Public Garden, Swan Boats, Zakim Bridge, Fenway-inspired details, historic brick architecture, harbor ferries, and the city’s waterfront atmosphere. Soft morning fog, golden spring light, subtle festive accents in crimson and gold, rich detail, layered depth, sophisticated city-poster aesthetics, fresh and refined, visually powerful but not overcrowded. Elegant typography in the lower left reads “SPRING 2026” with a vertical slogan “BOSTON, A CITY OF RIVER, MEMORY, AND INVENTION”, text clear and beautifully composed, premium graphic design, 9:16</pre>
</div>

### Case 4: Chinese Minimalist S-Shaped Poster

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>这类海报更接近“视觉构成实验”。S 形裂口、撕纸感、东方山水、留白、印章、排版，说明模型并不只是会出风格图，它也能吃下比较抽象的构图关系。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/liyue_ai/status/2045368305079447853">Chinese Minimalist S-Shaped Poster</a><br>
      作者：<a href="https://x.com/liyue_ai">@liyue_ai</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/poster_case4/output.jpg" alt="Chinese Minimalist S-Shaped Poster">
  </div>
</div>

<div class="gpt2-prompt">
<pre>极简新中式美学风格，画面以淡雅的灰白色为底，呈现出一种纸艺剪影般的立体感。
一条S形蜿蜒的裂痕状边缘将画面分割，仿佛撕开了一层纸面，露出内部色彩斑斓的东方山水景象。
裂口内，一条蜿蜒的河流自上而下贯穿整个构图，河水以深浅不一的蓝色渲染，层次分明，仿佛流动的丝带。
河岸两侧点缀着青翠的山丘与梯田，色彩柔和，绿红交织，展现出田园的宁静之美。
沿河而建的古风建筑错落有致，飞檐翘角，白墙黛瓦，在光影的映衬下更显古朴典雅。
岸边树木葱茏，枝叶轻盈，一艘小船静泊于水中央，增添了几分悠然意境。
整体构图呈S形曲线，富有韵律感，仿佛自然与人文的和谐共生。
画作边缘采用撕纸效果，营造出立体浮雕般的视觉体验。
下方题字“东方美学”以黑色楷体书写，日期“2026/04/18”与红色印章相呼应，底部“CHINA”字样庄重醒目，署名“@LIYUE”低调收尾，整体氛围静谧深远，充满诗意与哲思。</pre>
</div>

---

## 3. 角色设定

### Case 5: Persona5 Character Reference Card

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>角色设定图最怕只剩一张“好看的角色插画”。这个案例的价值在于，它明确要求了三视图、表情差分、装备拆解、色板和世界观说明，输出目标是一份资料卡，不是一张海报。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/iamrednightS/status/2045075682837836265">Persona5 Character Reference Card</a><br>
      作者：<a href="https://x.com/iamrednightS">@iamrednightS</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/character_case2/output.jpg" alt="Persona5 Character Reference Card">
  </div>
</div>

<div class="gpt2-prompt">
<pre>基于此角色和背景，请制作一份类似官方设定资料的角色资料卡。
・包含三视图：正面、侧面和背面
・添加角色面部表情的变化・分解并展示服装和装备的详细部分
・添加色板・包含世界观设定的简要说明
・总体上，使用有组织的布局（白色背景，插画风格）高分辨率、专业概念艺术风格</pre>
</div>

### Case 6: Mecha Girl Sea-City Key Visual

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>这个 case 很长，但不是废话。它几乎把角色设计里所有有用的维度都写了：人物材质、武器、站姿、世界观场景、光线、镜头、调色和最终成片风格。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/old_pgmrs_will/status/2046144801071079612">Mecha Girl Sea-City Key Visual</a><br>
      作者：<a href="https://x.com/old_pgmrs_will">@old_pgmrs_will</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/character_case7/output.jpg" alt="Mecha Girl Sea-City Key Visual">
  </div>
</div>

<div class="gpt2-prompt">
<pre>A mecha girl mid-teens, pale skin smudged with soot and salt spray, sharp amber eyes with glowing HUD reticles, waist-length ash-white hair tied in a high ponytail whipping in the sea wind, matte gunmetal exoskeleton armor plating her shoulders, forearms and shins, exposed hydraulic pistons at the joints, chest rig with glowing cyan coolant lines, oversized oil-stained hangar jacket half slipping off one shoulder, a massive rail cannon resting on her right shoulder, dog tags and frayed red ribbon at her collar, standing off-center to the left on the rusted edge of a tilted steel platform jutting out over dark water, weight shifted onto one leg, left hand gripping the cannon strap, head turned slightly toward camera with a quiet defiant stare, steam venting from her back thrusters, her ponytail and jacket streaming sideways in the salt wind, a vast derelict sea-city at dusk, colossal megastructures of unknown purpose rising from the ocean in staggered silhouettes, bone-white monolithic towers fused with barnacled steel, cyclopean ring-shaped constructs canted at broken angles, rusted skeletal gantries threaded with dead cables, dark swells rolling between the pylons, shipwrecks half-swallowed at their feet, thick sea fog clinging to the bases while the upper structures pierce into a bruised sky, scattered faint lights blinking high in the towers like distant eyes, moody low-key lighting, cold teal ambient from the overcast sky, warm amber sodium glow leaking from a distant structure camera-right, hard backlight from a low sun behind the towers carving her silhouette, volumetric god rays cutting through sea mist, wet specular highlights on her armor, 35mm anamorphic lens, slight low angle looking up past her shoulder toward the structures, medium-wide shot, shallow depth of field with foreground rust in soft focus, horizontal lens flares, fine atmospheric haze compressing the distant megastructures into layered silhouettes, cinematic anime key visual, painterly digital illustration with crisp line art, desaturated oceanic palette of teal, bone-white and rust punched by small warm accent lights, film grain, high-contrast editorial poster aesthetic. Format 16:9.</pre>
</div>

---

## 4. UI / 社交媒体截图

### Case 7: One-Prompt UI Design Generation

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>这条 prompt 很短，但方向是对的。它不是只要一个界面，而是要一整套系统：网页、移动端、卡片、控件、按钮。这是把“氛围图”转向“系统图”的关键一步。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/austinit/status/2044968740782272596">One-Prompt UI Design Generation</a><br>
      作者：<a href="https://x.com/austinit">@austinit</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/ui_case1/output.jpg" alt="One-Prompt UI Design Generation">
  </div>
</div>

<div class="gpt2-prompt">
<pre>用这种风格帮我生成一套UI设计系统，包含网页、移动端、卡片、控件、按钮 以及其它</pre>
</div>

### Case 8: Song Dynasty Social Media Feed

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>这类图的难点不是“像一个手机界面”，而是“界面规则和内容世界观同时成立”。它把宋代人物、梗、评论区、状态栏、图标系统都一起写进去了，所以更像一套完整的平行世界产品截图。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/Panda20230902/status/2045385588065313057">Song Dynasty Social Media Feed</a><br>
      作者：<a href="https://x.com/Panda20230902">@Panda20230902</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/ui_case4/output.jpg" alt="Song Dynasty Social Media Feed">
  </div>
</div>

<div class="gpt2-prompt">
<pre>"宋朝人的朋友圈"/"SONG DYNASTY SOCIAL MEDIA FEED"，古今穿越幽默融合界面设计风格，画面模拟手机社交媒体界面，但内容全部是宋朝场景头像是宋代文人画像，用户名"苏东坡SuShi_Official"，发布内容"刚到黄州，被贬了但心情还行。今天自己做了东坡肉，味道绝了，附菜谱："，配图为工笔画风格的东坡肉特写，点赞列表"黄庭坚、秦观、佛印等126人"，评论区"王安石：呵呵""司马光：还是那个味道"，界面元素如点赞图标用宋代花纹替代，状态栏显示"大宋移动 5G"和"元丰三年"，配色为手机深色模式搭配宋代雅致色调，历史与社交媒体的趣味碰撞杰作</pre>
</div>

---

## 5. 约束遵循与细节测试

### Case 9: Wooden Bookshelf Prompt Test

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>它看起来最朴素，但其实特别适合用来测试模型有没有真的理解局部规则。上层 1 本，中层 3 本，下层 7 本。这种 case 比“高级风格词”更适合测稳定性。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/chetaslua/status/2044331451077013749">Wooden Bookshelf Prompt Test</a><br>
      作者：<a href="https://x.com/chetaslua">@chetaslua</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/comparison_case5/output.jpg" alt="Wooden Bookshelf Prompt Test">
  </div>
</div>

<div class="gpt2-prompt">
<pre>A wooden bookshelf consisting of three shelves: On the top shelf, there should be one book, on the second shelf, there should be three books, and on the bottom shelf, there should be seven books.</pre>
</div>

### Case 10: GPT-Image-2 Detail Showcase

<div class="gpt2-grid">
  <div class="gpt2-card">
    <h3>为什么值得看</h3>
    <p>这个案例测的不是简单构图，而是多屏结构、季节主题切换、局部微观细节和文字元素共存。它很适合拿来观察模型在“复杂说明 + 连续视觉变体”上的稳定性。</p>
    <p class="gpt2-meta">
      原始案例：<a href="https://x.com/liyue_ai/status/2045000106919997637">GPT-Image-2 Detail Showcase</a><br>
      作者：<a href="https://x.com/liyue_ai">@liyue_ai</a>
    </p>
  </div>
  <div class="gpt2-card">
    <img src="https://raw.githubusercontent.com/EvoLinkAI/awesome-gpt-image-2-prompts/main/images/comparison_case10/output.jpg" alt="GPT-Image-2 Detail Showcase">
  </div>
</div>

<div class="gpt2-prompt">
<pre>以眼部特写图片为基础，生成3:4的四屏构图超写实眼部特写，四屏按春夏秋冬上下排序。

第一屏：眼眸中带着绽粉樱色的美瞳，睫毛缀满迷你春花，脸颊散落樱瓣与黄蕊小花，粉蝶萦绕眉眼，浅金发丝轻垂，下方簇簇樱花怒放，画面中央"SPRING"白色艺术字点缀，风格细腻唯美，光影柔和，色彩粉嫩治愈，下面用书法体写着春；

第二屏：眼眸中带着着清荷色的美瞳，睫毛饰以粉莲与绿荷，脸颊挂着晶莹水珠，粉瓣、绿荷点缀其间，蜻蜓轻绕，浅金发丝若隐若现，画面中央"Summer"白色艺术字凸显，光影通透流光感，色彩清透凉爽，下面用书法体写着夏；

第三屏：眼眸中带着金黄红相间的美瞳，睫毛饰以橙红枫叶，脸颊散落金红秋叶，橙蝶翩跹眉眼间，浅金发丝隐约可见，画面中央"AUTUMN"白色艺术字醒目，光影暖金流光，色彩浓郁温暖，下面用书法笔写着秋；

第四屏：眼眸中带着雪花蓝色的美瞳，睫毛覆满冰晶雪片，脸颊散落白色雪花与红色腊梅，银白蝴蝶翩跹眉眼，浅金发丝朦胧似雪，画面中央"WINTER"白色艺术字亮眼，光影冷冽蓝白流光，色彩清透纯净，下面用书法体写着冬。

整体呈现梦幻眼眸四季交替的唯美梦幻治愈画面，微调各屏的光影强度，让画面氛围感更浓郁。</pre>
</div>

---

## 最后我会怎么用这个仓库

如果只让我保留一个结论，我会这样用这份仓库：

<ul class="gpt2-list">
  <li>做人像时，重点抄镜头、光线、肤质和姿态的组合写法。</li>
  <li>做海报时，重点抄视觉路径、留白和文字区位置。</li>
  <li>做角色设定时，重点抄“三视图 + 差分 + 色板 + 世界观”这种资料卡结构。</li>
  <li>做 UI 截图时，不要只写风格，要写界面类型、模块和内容。</li>
  <li>做测试时，拿书架计数、四屏细节、信息图这类 case 去测模型的遵循能力。</li>
</ul>

<p class="gpt2-note">
这篇文章里的 prompt、结果图和来源链接均来自 EvoLinkAI 的开源仓库整理。我做的是精选和重组，不是原文镜像。完整案例库请直接看原仓库：
<a href="https://github.com/EvoLinkAI/awesome-gpt-image-2-prompts">awesome-gpt-image-2-prompts</a>。
</p>
