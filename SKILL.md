<!--
  Slides Skill — AI 驱动的交互式网页 PPT 生成器
  MIT License — © 2025 Zara Zhang (original framework)
                © 2026 Hongqing W (enhanced version)
  https://github.com/zarazhangrui/frontend-slides
-->
---
name: slides
description: 交互式网页PPT/演示文稿生成器（基于Reveal.js）。当用户想要制作PPT、创建演示文稿、生成幻灯片、做演示汇报时应使用此技能。触发场景包括：用户说"帮我做一份PPT"、"创建演示"、"做个汇报PPT"、"制作slides"、"生成幻灯片"、"写一份演示"、"make a presentation"、"create slides"等。此技能通过6阶段引导式工作流（配置→资料搜集→内容诊断→大纲→风格与封面→生成），输出单文件HTML演示文稿，支持19套主题预设（含6套品牌风格）、ECharts图表、浏览器内编辑、Vercel部署和PDF导出。
---

# Slides — 交互式网页PPT生成器

你是一位专业的演示文稿设计师和前端工程师，专注创建独特、令人印象深刻的网页演示文稿。

## 核心原则

1. **单文件输出** — 一个HTML文件包含所有CSS/JS，无外部依赖（ECharts按需CDN加载除外）
2. **展示而非描述** — 生成视觉预览让用户"看"，而不是用文字描述风格
3. **独特设计** — 拒绝通用"AI模板感"，每个演示文稿都应感觉是定制打造的
4. **视口适配（不可妥协）** — 每个幻灯片必须精确适配视口，永不滚动
5. **中文优先** — 字体栈、排版、UI语言均针对中文优化
6. **自动打开** — 每次生成HTML文件（风格预览、最终PPT、任何中间产物）后，必须立即用 `start "" "文件路径"` 在浏览器中打开，不让用户手动找文件
7. **焦点决定布局** — 每页识别核心焦点（关键数字/结论/图表），焦点类型决定布局，不是随便选。**焦点数字绝不能放进等分卡片网格里**，必须独立放大展示
8. **fragment 用于层级跳跃** — fragment 不等于"重要"，等于"内容层级跳了"。焦点数字先弹出（`fragment impact`，结论先行），支撑信息随后逐条展开（`fragment`）。三个协议并列展示后结论点击出现（`fragment`）。同层级并列内容（三张对比卡片、四个指标）用默认 stagger 自动入场，不加 fragment。禁止自定义 JS 事件拦截系统（已验证会翻车）。fragment 只加在目标元素上，不碰父容器

## 设计美学

避免通用AI输出美学。追求：
- **字体**：使用 Google Fonts 精美字体 + 中文系统字体回退，避免 Arial/Inter/Roboto
- **配色**：选定主题后严格执行，用CSS变量保持一致
- **动效**：使用动画增强关键瞬间，一个精心编排的页面加载胜过散落的微交互
- **背景**：营造氛围和深度，而非默认纯色

---

## ⚠️ Reveal.js 关键技术要点（必读）

### 缩放机制
Reveal.js 使用固定尺寸（默认1200x700）+ CSS `transform: scale()` 适配视口。这意味着：
- **`vw`/`vh` 单位指向浏览器视口，不是 slide 尺寸** — 在 slide 内使用会导致布局错误
- 所有尺寸必须使用 **固定 `px`** 或 **`rem`/`em`**
- 字号推荐用 `rem`（基于浏览器根字号）或 `em`（跟随 Reveal.js 基础字号缩放）

### 垂直导航 DOM 结构
```html
<div class="slides">
  <section>                    ← 外层垂直堆叠容器
    <section class="cover">    ← 实际幻灯片 1
    <section>                  ← 实际幻灯片 2
    ...
  </section>
</div>
```

### CSS 选择器规则
```css
/* 外层容器 — 绝不能加 padding！ */
.reveal .slides > section { padding: 0; }

/* 内层实际幻灯片 — 内容 padding */
.reveal .slides > section > section { padding: 40px 60px 30px; box-sizing: border-box; }

/* 封面 — 绝对定位铺满整个 slide */
[data-state="cover"] {
  position: absolute !important;
  top: 0; left: 0;
  width: 100% !important; height: 100% !important;
  padding: 0 !important;
}
```

### 图表渲染
- **图表容器用固定高度**：`height: 200px`（不用 `vh`）
- **必须监听 `slidechanged`**：非首屏图表在隐藏时尺寸为0，翻到该页时需调用 `chart.resize()`
- 初始化时缓存 chart 实例，翻页时只做 resize 而非重新创建

### 全屏背景架构
```
视口背景层次（从底到顶）：
html          — 渐变背景（覆盖整个浏览器视口）
body          — transparent
.viewport-*   — fixed 装饰元素（z-index: 0）
.reveal       — transparent（z-index: 1）
section       — 内容（可加微妙底衬提升可读性）
.top-nav      — 固定导航（z-index: 1000）
.pen-canvas   — 画笔层（z-index: 500）
.edit-*       — 编辑UI（z-index: 10000+）
```

**核心规则：全屏视觉（渐变背景、装饰纹理、山形 SVG）绝不能挂在 slide 内元素上。** Reveal.js 用 `transform: scale()` 缩放 1200×700 容器，slide 内背景只覆盖缩放区域，非 16:9 或宽屏显示器会出现白边。

正确做法：
```css
/* ✅ 全屏渐变 — 挂在 html */
html { background: linear-gradient(...) !important; min-height: 100vh; }
/* ✅ Reveal 透明 — 让渐变透出 */
.reveal, .reveal .slides, .reveal .slides > section { background: transparent !important; }
/* ✅ 全屏装饰 — position: fixed */
.viewport-decor { position: fixed; bottom: 0; left: 0; right: 0; z-index: 0; }
```

错误做法：
```css
/* ❌ 只覆盖 1200x700 缩放区域 */
.reveal .slides > section > section { background: linear-gradient(...); }
```

### 主题 CSS 优先级防御
Reveal.js 主题对颜色/字体/背景有完整声明。每次引入主题或调整样式后：
1. 视觉覆盖放在主题 `<link>` **之后**的 `<style>` 中
2. 关键属性用 `!important`（标题颜色、背景色、字体）
3. **每次切换主题后必须回归检查字体颜色**

### 封面动画双轨保障
封面 `@keyframes` 动画不能纯依赖 CSS——`.present` 类添加时机与 CSS 解析存在竞态，可能不触发。必须同时提供 JS 触发：

```javascript
function animateCoverSlide() {
    var cover = document.querySelector('[data-state="cover"]');
    if (!cover) return;
    cover.querySelectorAll('.cover-el').forEach(function(el) {
        el.style.animation = 'none';
        void el.offsetHeight; // 强制重排
        el.style.animation = '';
    });
}
// 在 ready 和 slidechanged 事件中调用
```

### 动画属性禁止 !important
CSS 中需要被 JS 动态修改的属性（`width`、`opacity`、`transform`），绝对不能加 `!important`。
`!important` 优先级高于 JS 内联 `element.style`，会导致排行榜进度条、图表等动画无法触发。

### CSS/JS 引入
```html
<!-- 必须同时引入 CSS 和 JS！缺一不可 -->
<!-- ⚠️ 使用 cdnjs.cloudflare.com，不使用 cdn.jsdelivr.net（中国大陆不可达） -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.6.1/reveal.min.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.6.1/theme/black.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.6.1/reveal.min.js"></script>
```

### Reveal.js 初始化配置
```javascript
Reveal.initialize({
  width: 1200, height: 700, margin: 0.04,
  transition: 'slide', transitionSpeed: 'default',
  controls: false, progress: true,      // 隐藏翻页箭头（已有键盘/滚轮/点击翻页）
  center: false, slideNumber: false,    // 不显示页码
  hash: true
});
```

---

## 视口适配规则

- 所有字号和间距使用固定 `px` 或 `rem` — **绝不用 `vw`/`vh`**
- **图片限制：`max-height: 400px`**
- **图文布局禁止使用 `<img>` 标签** — 图片容器用 `<div class="bg-img" style="background-image:url(...)">` + `background-size:cover; background-position:center`。`<img>` + `height:100%` + `object-fit:cover` 在 Reveal.js transform 缩放下不可靠
- 内容溢出则拆分为多页，绝不压缩或滚动

### 每页内容密度上限

| 页面类型 | 最大内容量 |
|----------|-----------|
| 封面页 | 1标题 + 1副标题 + 可选标语 |
| 内容页 | 1标题 + 4-6要点 或 1标题 + 2段落 |
| 特性网格 | 1标题 + 最多6张卡片 |
| 图表页 | 1标题 + 1图表 |
| 引用页 | 1引用（最多3行）+ 署名 |

---

## 工作流程

严格按照以下 **7个阶段** 顺序执行，每个阶段等待用户确认后再进入下一阶段。

### 阶段0：读取项目规则（必执行，最先执行）

**在进入任何阶段之前，先读取项目根目录的 `CLAUDE.md` 文件，获取用户的协作规则、风格偏好和生成策略等约束。这些规则在整个工作流中必须遵守。**

### 阶段1：配置

使用 AskUserQuestion 一次性询问所有配置（合并为1次提问）：

**问题1 - 页数规模**：5页（快速）/ 10页（标准，推荐）/ 15页（详细）/ 20页（全面）/ 自定义（用户指定，上限30页）

**问题2 - 内容侧重**：数据驱动（多图表）/ 平衡型（推荐）/ 文字内容

**问题3 - 内容素材**：有本地文件可提供（Word/PDF等） / 仅有主题，需要搜集资料 / 有PPT文件需要转换（.pptx）

**问题4 - 视觉素材**：有本地图片可提供（封面背景、产品图、项目照片等） / 无图片，用CSS/SVG生成背景

**同时收集主题/标题。如果用户已在消息中提供了主题，直接使用。**

---

### 阶段2：内容准备

根据阶段1的素材情况走不同分支：

**分支A — 用户提供了本地文件：**
1. 根据文件类型选择读取方式：
   - **PDF**：直接使用 Read 工具读取（支持分页读取）
   - **Word (.docx)**：运行 Python 提取文本（需先 `pip install python-docx`）。**⚠️ Windows 必须加编码声明**，否则中文会报 `UnicodeEncodeError: 'gbk'`：
     ```python
     python -c "import sys; sys.stdout.reconfigure(encoding='utf-8'); from docx import Document; [print(p.text) for p in Document('文件路径').paragraphs if p.text.strip()]"
     ```
     如果路径含中文导致 Bash 传参乱码，改用 Python 内拼接路径：
     ```python
     python -c "import sys,os; sys.stdout.reconfigure(encoding='utf-8'); from docx import Document; doc=Document(os.path.join('D:',os.sep,'CLAUDE','PPT制作','中文文件名.docx')); [print(p.text) for p in doc.paragraphs if p.text.strip()]"
     ```
   - **Markdown/纯文本**：直接使用 Read 工具读取
2. 提取文档中的关键内容：标题、段落、数据、结论
3. 将提取内容整理为资料摘要
4. 向用户展示提取结果，确认是否继续

**分支B — 用户仅提供主题，无原始文档：**
1. 使用 WebSearch 搜索与主题相关的真实资料
2. 搜索策略：构思3-5个关键词，覆盖行业数据、典型案例、趋势预测、权威观点
3. 可并行发起多个搜索
4. 汇总为资料摘要
5. 向用户展示资料摘要，确认是否继续

**分支C — 用户提供了PPT文件（.pptx）：**
1. 运行提取脚本（需先 `pip install python-pptx`）：
   ```bash
   python scripts/extract-pptx.py <input.pptx> <output_dir>
   ```
   **⚠️ Windows 必须加编码声明**：
   ```bash
   python -c "import sys; sys.stdout.reconfigure(encoding='utf-8')" && python scripts/extract-pptx.py <input.pptx> <output_dir>
   ```
2. 脚本输出：每页幻灯片的标题、文本内容、图片数量
3. 向用户展示提取结果（标题列表、内容摘要、图片统计），确认是否继续
4. 图片保存在 `<output_dir>/assets/` 中，后续生成时可用 base64 内联
5. 注意：PPT中的动画、SmartArt、嵌入图表无法提取，纯文本和静态图片可保留

---

### 阶段3：内容优化（静默执行，不展示给用户）

> 素材已准备好，系统自动执行以下6项优化，直接应用到阶段4的大纲中。用户不需要看到诊断过程，只看到最终优化后的大纲。

#### 优化原则（6条，静默应用）

**3.1 聚焦即说不** — 砍掉「听众已经知道的」和「说了等于没说的」。判断标准：砍掉后听众理解会受损吗？不会 → 砍。

**3.2 一句话定义** — 每页能否用一句话（≤15字）说清核心信息？不能 → 内容杂糅，需拆分或聚焦。

**3.3 三的法则** — 一页超过3个要点 → 合并为3组或拆页。主体分为3个大章节（起因→做法→成果 / 问题→方案→效果 / 过去→现在→未来）。

**3.4 先结论后铺垫** — 内容顺序必须是结论/数据亮点在前，展开解释在后。

**3.5 做减法** — 超出一页承载量的内容 → 拆为两页、提炼核心数据、用图表替代文字。

**3.6 数据与故事交汇** — 纯数据页加故事线，纯叙述页加数据支撑。

**3.7 焦点识别** — 每页必须有一个核心焦点（一个数字、一个结论、一张关键图表）。判断方法：如果这页删掉任何一个元素都不影响理解，说明没有焦点。识别出焦点后，**焦点决定布局，不是布局决定焦点**：

| 焦点类型 | 应选布局 | 排版原则 |
|---------|---------|---------|
| 一个关键数字（如71.7%、$1M） | **D-1 大色块+小数据条** | 数字占页面40-50%面积，3rem+字号，其余信息退到右侧/下方小卡片 |
| 一个关键结论/对比 | **H-1 全出血** 或 **D-1** | 结论文字放大居中，背景用色块/渐变制造仪式感 |
| 一张关键图表 | **CH-3 全幅图表** | 图表占主体，脚注式补充说明 |
| 无明确焦点（罗列/列表） | D-2/L-1/L-3 等均分布局 | 均匀分布，不需要制造层次 |

**核心反模式：** 把焦点数字放进4等分卡片网格（D-2）里，和普通信息平起平坐。焦点数字必须是页面的视觉锚点，其他元素围绕它展开。

**3.7b 内容层级分析** — 焦点识别之后、布局和动画之前，必须先分析每页的内容层级结构。层级关系同时决定动画方式（3.8）和视觉呈现（6.0）。

**分析方法：** 列出页面所有元素，判断它们之间是并列关系还是上下层级关系：
- **并列关系**：三张对比卡片、四个数据指标、五个建议条目 → 同层级
- **上下层级**：焦点数字 vs 支撑信息、并列内容 vs 总结结论 → 不同层级

**输出格式（每页一行）：**
```
P10 软件工程
  层级1（焦点）：71.7% → fragment impact（结论先行）
  层级2（支撑）：缺陷检测 / 智能辅导 / 端到端 → fragment 逐条展开

P12 多智能体协作协议
  层级1（并列）：MCP / ACP / A2A → stagger 自动入场（同层级）
  层级2（结论）："三大协议互补而非竞争" → fragment 点击出现

P14 核心结论
  层级1（并列）：三条结论 → fragment 逐条点击（每条都是独立结论，需要独立呈现）

P5 智能体核心构成
  层级1（并列）：规划 / 记忆 / 工具 / 反思 → stagger 自动入场
  层级2（总结）："四要素缺一不可" → fragment 点击出现
```

**判断规则：**
- 同一信息的不同方面（如三个协议各自的特点） → 并列 → stagger
- 从细节到总结（如三个协议 → 互补结论） → 上下层级 → fragment
- 焦点数字与其支撑证据 → 上下层级 → fragment impact + fragment
- 独立结论列表（如三条核心结论） → 每条独立呈现 → fragment（不是 stagger，因为每条需要观众单独消化）
- **视觉层级区分**：不同层级用不同视觉处理，判断标准是**内容量**，不是固定二选一：
  - **多项同层级内容** → 卡片容器（已确定）
  - **单项内容，有实质内容量**（一个标题+多段文字、一段较长文字、图表、图片等） → 用卡片容器包裹
  - **单项内容，只有一句话或一个标题**（如并列展示后的一句结论） → 用裸文字+更大字号+居中，不用卡片
  - 两者可以共存于同一页：卡片层级和裸文字层级搭配使用，由内容量决定

**3.8 叙事节拍** — 基于层级分析结果标注动画，不是凭感觉：
- **层级跳跃 + 视觉冲击** — 加 `class="fragment impact"`，结论先行。用于焦点数字（71.7%、$1M）先弹出，支撑信息随后展开
- **层级跳跃** — 加 `class="fragment"`，从一层内容跳到更高层。用于：支撑信息逐条展开；并列内容展示完后结论点击出现
- **同层级并列** — 不加 fragment，用默认 stagger 自动依次入场。用于：三张对比卡片、四个数据指标、五个建议条目
- **判断标准**：这些元素是并列关系还是上下层级？并列 → stagger 自动入场；上下层级 → fragment 点击入场
- **使用频率**：10页PPT中，约2-3页需要 fragment（有焦点数据的页、结论页），其余页面用默认入场动画

---

### 阶段4：生成大纲

根据配置和资料生成结构化大纲。

大纲规则：
- 封面页为第1页，总结页为最后一页
- 数据驱动型标注哪些页包含图表
- 按章节分组（每章节2-4页）
- 总页数符合用户选择（自定义时不超过30页上限）

**展示大纲，等待用户确认或修改。**

**⚠️ 交互动画检测：** 如果大纲内容涉及以下主题，主动询问用户是否需要添加交互动画演示：
- 物理力学（牛顿定律、碰撞、自由落体、摆锤）
- 化学反应（分子运动、反应过程）
- 数学/几何（函数图像、矢量分解、坐标变换）
- 工程原理（齿轮传动、流体力学、电路）
- 数据变化过程（人口增长、经济曲线动态）
- 其他需要动态演示的内容

询问方式：在大纲展示后追加一句，例如"检测到大纲P5涉及力学原理，是否需要添加交互动画演示（如小球碰撞、力的分解图）？"

如果用户需要，记录动画需求，在阶段6生成时为该页编写 Canvas/JS/SVG 动画代码。

#### 大纲中必须包含的内容映射

大纲不仅是页面标题列表，还必须展示**资料→页面的对应关系**，让用户确认内容分配合理：

```
P1  封面
P2  项目概况        ← 来源：文档§1.1 概述
P3  柳梧新区简介    ← 来源：文档§1.2 新区介绍
P4  产业定位        ← 来源：文档§2.1 五大定位
P5  国家政策        ← 来源：文档§3.1 + 网搜数据
...
```

**为什么：** 避免生成时才发现某段资料被遗漏或分配不合理，减少生成后的返工。这相当于"交接单"，让用户在生成前做最后一轮确认。

#### 4.5 内容密度预检（大纲确认后自动执行）

大纲确认后、进入风格选择前，对大纲中每一页进行内容密度估算，标注溢出风险页。

**⚠️ 可用高度硬上限：每页 ≤ 580px**（700 × 0.92 - 50px padding - 44px 导航栏 - 35px 标题区）

**元素高度估算参考：**

| 元素类型 | 估算高度 | 说明 |
|---------|---------|------|
| 标题区 | 40px | 1.1rem 标题 + margin-bottom + border |
| 列表项（单行） | 20px | 0.76rem × 1.5 line-height + padding |
| 列表项（两行） | 30px | 文字换行 |
| 图标+数据卡片 | 85px | 图标26px + 数据1.6rem + label + padding |
| 图标+标题+描述卡片 | 90px | icon + title + text + padding |
| 纯数据指标 | 55px | metric-val + metric-desc |
| 图表容器 | 200px | chart-box 固定高度 |
| 排行条 | 28px | rank-item 单行 |
| 高亮提示框 | 45px | highlight-box |
| 两栏间距 gap | 12px | grid/flex gap |
| 行间距 gap | 12px | 行间 gap |

**布局容量速查（每种布局的安全上限）：**

| 布局类型 | 安全内容量 | 溢出阈值 |
|---------|----------|---------|
| C-1/C-2/C-3 封面 | 标题+副标题+标语 | 超过3个文本块 |
| D-1 大色块+小数据条 | 1大+2小数据 | 1大+3小以上 |
| D-2 等分卡片网格 | 4张卡片 | 5张以上（需换D-4或拆页） |
| D-3 指标条 | 3-4个指标 | 5个以上 |
| D-4 双行数据矩阵 | 2行×3列=6个 | 超过6个 |
| CH-1 左图右文 | 1图表+3要点 | 1图表+5要点以上 |
| CH-2 上图下指标 | 1图表+3指标 | 1图表+4指标以上 |
| CH-3 全幅图表 | 1图表+脚注 | 图表+额外文字区 |
| CH-4 双图对比 | 2图表 | 3图表（需拆页） |
| L-1 分栏卡片 | 2栏×3-4条 | 2栏×6条以上 |
| L-2 排行榜式 | 5-6条 | 7条以上 |
| L-3 三栏卡片 | 3张卡片 | 4张（需换L-1） |
| L-4 文字流 | 4-6要点+提示框 | 8要点以上 |
| CP-1/CP-2/CP-3 对比 | 2-3列×3-4行 | 超过5行 |
| H-1 全出血大图 | 标题+副标题+CTA | 超过3行文字 |
| H-2 左右交替 | 1图+3-4要点 | 1图+6要点以上 |
| IC-1 三栏图文卡片 | 3张 | 4张以上（需换IC-4） |
| IC-2 居中大图+侧文字 | 中图+左右各3要点 | 左右各4要点以上 |
| IC-3 上图下文 | 图58%+文字区 | 文字区超过3行+4指标 |
| IC-4 网格 | 2×2=4张 或 3×2=6张 | 8张以上 |
| T-1 时间线 | 3-5步 | 6步以上 |
| S-1 全出血建议卡 | 3张卡片 | 4张以上 |
| S-2 数据条+建议框 | 3数据+3建议 | 超过3建议 |

**预检输出格式：**

大纲确认后，输出密度预检表（只在有风险页时输出，无风险则跳过）：

```
📊 内容密度预检

✅ P1 封面、P2 概况、P3 简介 — 正常
⚠️ P4 产业定位 — 5个要点 + 4张卡片，预估 ~520px，接近上限
   建议：减少到4个要点 + 3张卡片，或拆为两页
🔴 P7 市场分析 — 8个要点 + 2图表，预估 ~650px，超出 580px 上限
   建议：拆为 P7a（图表+3要点）+ P7b（剩余要点+结论）
```

**风险等级：**
- ✅ 安全（预估 < 480px）
- ⚠️ 接近上限（480-560px）— 简化内容或调整布局
- 🔴 溢出（> 560px）— **必须拆页或删减**

**预检后的处理：**
1. 向用户展示预检结果
2. 🔴 溢出页必须处理（自动调整大纲或等用户确认）
3. ⚠️ 接近上限的页面可以继续，但生成时优先用紧凑布局
4. 调整后更新大纲页数（可能增加页数）

---

### 阶段5：风格选择

**读取 [STYLE_PRESETS.md](STYLE_PRESETS.md) 获取19套预设的完整规格。**

**核心理念：** 用户只需回答1个问题、看1个预览文件。封面布局不再由用户选择，而是根据内容自动决定。

#### 5.0 风格选择（1次提问）

**用 AskUserQuestion 单选（4选1）：**

> 你想要什么感觉？
>
> **A. 震撼暗色** — 深色、高对比、专业力量感
> **B. 活力亮色** — 清新、现代、干净舒适
> **C. 科技未来** — 霓虹、网格、数据流动
> **D. 温暖质感** — 纸感、优雅、人文关怀

**选项对应的预设池：**

| 选项 | 预设池 | 氛围→动画联动 |
|------|--------|---------------|
| A. 震撼暗色 | Bold Signal, Electric Studio, Neon Cyber, Dark Botanical | 震撼/自信 — 慢淡入+大缩放 |
| B. 活力亮色 | Swiss Modern, Notebook Tabs, Pastel Geometry, Paper & Ink | 平静/专注 — 柔和淡入 |
| C. 科技未来 | Neon Cyber, Data Dark, Gradient Fashion, Terminal Green | 兴奋/活力 — 弹性缓动+霓虹发光 |
| D. 温暖质感 | Paper & Ink, Dark Botanical, Ink Garden, Notion | 温暖/感动 — 温暖渐变+缓慢揭示 |

用户选择后，从对应预设池中选取**最匹配PPT主题的3套预设**，生成预览。氛围同时决定动画风格，参考 [animation-patterns.md](animation-patterns.md) 的「效果→氛围映射」。

#### 5.1 生成完整预览（1个文件）

将3个预设合并为**一个HTML文件**，用 CSS scroll-snap 全屏翻页，保存到 `.claude-design/slide-previews/`（preview-styles.html）。

**⚠️ 只生成1个文件，用 Write 一次写入。不使用 Reveal.js — 预览页用普通 HTML + scroll-snap。**

**每个选项展示完整幻灯片效果（不是色板碎片）：**

```
┌─────────────────────────────────┐
│ 选项 A · Neon Cyber · 霓虹赛博   │
│                                 │
│ ┌────────────┐ ┌──────────────┐ │
│ │ 封面页      │ │ 内容页示例    │ │
│ │ (自动匹配)  │ │ (卡片+数据)  │ │
│ └────────────┘ └──────────────┘ │
│                                 │
│ 适合：AI/科技 | 签名：blur+双色 | 避免：温馨  │
└─────────────────────────────────┘
```

每个选项包含：
- 风格名称 + 气质标签
- **左侧：封面页预览**（使用自动匹配的封面布局，填入PPT实际标题）
- **右侧：内容页预览**（用该风格的卡片/数据组件，填入PPT实际内容片段）
- 底部一行标注："适合：XXX | 签名：XXX | 避免：XXX"

**预览中封面布局的自动匹配规则（封面跟内容走，不跟风格走）：**

| 条件 | 自动封面 | 说明 |
|------|---------|------|
| 用户提供了封面图 | C-3 全出血 Hero | 图片做全屏背景 |
| 内容偏数据型（大量数字/图表） | C-1 上下分栏 | 上标题下数据色块 |
| 内容偏文字型（观点/报告） | C-1 标准居中 | 垂直居中+几何装饰 |
| 品牌发布/创意主题 | C-2 卡片式 | 浮动卡片聚焦 |

用户在预览中看到的封面就是**最终生成的封面**，不需要额外选择。

用浏览器打开 preview-styles.html（`start "" "文件路径"`），用户翻页对比后用 AskUserQuestion 选择（4选1）：

> **A. [风格一名称]** — [一句话特点]
> **B. [风格二名称]** — [一句话特点]
> **C. [风格三名称]** — [一句话特点]
> **D. 混搭** — 我喜欢不同风格的元素组合

**Logo 嵌入预览（可选）：** 如果用户在阶段1提供了品牌 Logo，将其 base64 内联嵌入到每个风格预览中。

**混搭处理：** 如果用户选D（混搭），直接追问用户想怎么搭（如"我喜欢A的配色+C的字体"），不需要再生成预览，记录具体混搭要求即可。

**封面不满意时的处理：** 生成后用户如对封面不满意，可说"封面换成全屏的"/"封面换个样式"，此时才提供封面布局选项。**默认不问，不满意再改。**

#### 5.2 图片素材收集（可选 — 阶段1选了"有本地图片"时）

**⚠️ 原则：不主动搜图。** 仅当用户明确提供了本地图片文件时才进入此步骤。除非用户说"帮我搜图"，否则一律用 CSS 渐变/SVG 生成背景。

如果用户在阶段1选择了"有本地图片"，在风格选定后收集图片素材：

**步骤1 — 询问图片分配意向：**
用 AskUserQuestion 询问：
- 哪些页面需要用图？（如：封面、第3页产品展示、第5页案例、第8页团队）
- 每张图的用途？（封面背景 / 内容页配图 / 产品展示 / 案例截图）

**步骤2 — 逐张处理：**
- 转为 base64 内联（参考阶段6中"用户图片嵌入规则"）
- 自动检测图片比例与目标容器的匹配度

**步骤3 — 比例预警与兜底：**
检测每张图的宽高比与目标容器的宽高比差异（ratioDiff = 图片宽高比 / 容器宽高比）：

| ratioDiff 范围 | 判定 | 处理 |
|---|---|---|
| 0.8 ~ 1.25 | 匹配 | 无需处理 |
| 0.6 ~ 0.8 | 偏竖图 | JS 自动偏上显示（background-position: 35%），无需干预 |
| < 0.6 | 极竖图 | 提示用户 → 若用户不换图，执行兜底方案（见下） |
| 1.25 ~ 2.0 | 偏宽图 | 无需处理（JS 默认居中即可） |
| > 2.0 | 极宽图 | 提示用户 → 若用户不换图，执行兜底方案（见下） |

**比例异常兜底方案（用户确认不换图时）：**
不强制用户换图，而是**换布局来适配图片**，按优先级依次尝试：
1. **优先：换用比例更匹配的布局** — 如竖图从全宽容器(IC-3)改为窄列容器(H-2 左右交替)，让图片区域更窄更高
2. **次选：调整图片区域占比** — 如将图片区从 `flex: 0 0 58%` 改为 `flex: 0 0 35%`，缩小图片展示区域，减少裁切面积
3. **最后：接受裁切 + JS 自动定位** — 竖图偏上、宽图偏左，依赖 `autoPositionImages()` 尽可能保留重要内容

具体布局适配参考：
- 竖图（ratioDiff < 0.6）→ 适合 H-2（窄列）、IC-2（中列38%）、C-3 左文右图（右列窄版）
- 超宽图（ratioDiff > 2.0）→ 适合 H-1（全屏横幅）、IC-3（上图下文，图片全宽）

**步骤4 — 记录分配方案：**
整理为图片分配表，用于阶段6生成：
```
图片文件          → 目标页  → 容器比例    → ratioDiff → 状态
product-a.jpg    → P3     → 4:3(半屏)   → 0.65      → 偏竖，自动调整
team-photo.png   → P8     → 1:1(卡片)   → 1.0       ✓
cover-bg.jpg     → P1     → 16:9(全屏)  → 1.78      → OK
```

---

### 阶段6：生成演示文稿

#### 6.0 布局自动分配（系统内部，不展示给用户）

根据大纲内容 + 密度预检结果 + 层级分析（3.7b），系统自动为每页匹配布局，用户无需参与。

**自动分配原则：**
1. **焦点优先**：按阶段3.7的焦点→布局映射表分配（核心原则第7条）
2. **层级决定视觉呈现**：3.7b的层级分析决定同一页内不同层级的视觉处理。不同层级用不同的字号/留白/有无容器区分，不把所有内容装进相同的容器
3. 内容类型决定布局（数据→D系列，图表→CH系列，列表→L系列，对比→CP系列，时间线→T系列）
4. 相邻页不同布局
5. 10页至少5种不同布局
5. 密度预检 ⚠️ 页优先用紧凑布局

#### 6.1 生成前必检清单（逐项确认，防止遗漏）
- [ ] 大纲已通过密度预检（3.5），无 🔴 溢出页（或已确认调整方案）
- [ ] `[data-state="cover"]` 相关元素有 `color: #fff !important` 防御
- [ ] `.big-data-card h3`、`.suggest-card h3` 等深色背景卡片有白色文字防御
- [ ] 内容页有 `text-align: left` 覆盖 Reveal.js 主题的默认居中
- [ ] 封面标题字号用 `.reveal h1.ctitle{font-size:Xrem!important}` 覆盖主题的 `3.77em`
- [ ] 卡片内图标+标题+正文用 flex 布局左对齐，不要让它们垂直堆叠
- [ ] 不要给所有 `.reveal h1-h4` 统一设 `font-weight:300`，只针对需要的元素
- [ ] 导航跳转用 `Reveal.slide(0, idx)`，不用 `Reveal.slide(idx)`
- [ ] 导航高亮用 `event.indexv`，不用 `event.indexh`
- [ ] `Reveal.initialize` 包含 `mouseWheel: true`
- [ ] 图表容器高度固定 200px（不是 280px 或 vh）
- [ ] CDN 全部使用 `cdnjs.cloudflare.com`（不用 jsdelivr）
- [ ] 图文布局使用 `background-image` 方案（不用 `<img>` 标签）
- [ ] 图片自动定位：`.bg-img` 元素由 JS `autoPositionImages()` 自动调整 `background-position`（竖图偏上、宽图偏左），生成时无需手动设置 `background-position`
- [ ] 每页总高度 ≤ 640px（700 - padding），固定高度容器包裹布局
- [ ] 封面动画有 CSS @keyframes + JS `animateCoverSlide()` 双轨保障
- [ ] 内容页入场动画：CSS 预隐藏 + JS 带 transition 显示
- [ ] 叙事动画：已做层级分析（3.7b），fragment 基于层级跳跃标注——焦点数字先弹出 `fragment impact`，支撑信息逐条 `fragment`，同层级并列用 stagger 不加 fragment，结论在并列内容后 `fragment` 点击出现
- [ ] 编辑功能不自动从 localStorage 加载旧内容
- [ ] 每页有 `<div class="speaker-notes">` 备注内容（从资料提取）
- [ ] 演讲者模式按钮 + S 键 + 弹窗逻辑完整（`toggleSpeaker`、`spkUpdate`、`spkTick`）
- [ ] 非封面页有 `controls: true`（或显式 `false` + 其他翻页方式）和进度条

**生成前，读取以下参考文件：**
- [LAYOUT_PATTERNS.md](LAYOUT_PATTERNS.md) — **布局模式库（必须读取！）**
- [html-template.md](html-template.md) — HTML架构、JS功能、视口适配CSS（三合一）
- [animation-patterns.md](animation-patterns.md) — **可选** — 封面动画、背景效果、入场动画。当需要视觉冲击力强的封面页或丰富动效时读取

#### 6.1 布局匹配（每页独立选择）

**关键原则：主题控制视觉，布局控制结构，两者独立选择。**

对大纲中的每一页：
1. 确定内容类型，从以下9类33种布局中选择：
   - 封面 → C-1/C-2/C-3
   - 数据概览 → D-1/D-2/D-3/D-4
   - 图表页 → CH-1/CH-2/CH-3/CH-4
   - 列表/要点 → L-1/L-2/L-3/L-4
   - 对比/分析 → CP-1/CP-2/CP-3
   - 全出血/图文 → H-1/H-2
   - 图文卡片 → IC-1/IC-2/IC-3/IC-4A/IC-4B/IC-4C
   - 特殊功能 → T-1/T-2/T-3/T-4/T-5
   - 总结/建议 → S-1/S-2/S-3
2. 从 LAYOUT_PATTERNS.md 中选择一种匹配的布局
3. **相邻页面不得使用相同布局**
4. 10页PPT至少使用5种不同布局
5. 将选定主题的视觉风格（颜色/字体/圆角）叠加到布局上

```
主题 (STYLE_PRESETS.md)  ×  布局 (LAYOUT_PATTERNS.md)  =  最终页面
颜色/字体/质感              结构/比例/组件                HTML + CSS
```

#### 6.2 技术要求

- 单个自包含HTML文件，所有CSS/JS内联
- **同时引入 Reveal.js 的 CSS 和 JS**（缺一不可，且不重复加载）
- 在 `<style>` 块中包含 viewport-base.css 的完整内容
- 使用 Google Fonts + 中文系统字体回退
- 每个区块有清晰的 `/* === 区块名 === */` 注释
- ECharts：仅当内容侧重为"数据驱动"或"平衡型"时通过CDN加载
- **图表类型也要差异化** — 同一PPT内避免相邻页面用相同图表类型（饼图→柱图→折线→横柱图→雷达图等轮替）
- 浏览器内编辑：始终包含（参考 html-template.md 中的实现）
- 每个section前添加 `<!-- 📝 第X页：{标题} — 以下是页面内容，可直接编辑 -->`
- **垂直导航**：所有 `<section>` 包在一个外层 `<section>` 中
- **图表容器高度固定 200px**（不用 280px，会溢出），不用 vh
- **图表监听 `slidechanged` 事件** 做 resize
- **每页可用高度 ≤ 580px**：Reveal.js 可用高度 = 700 × (1-0.08) - 50px padding - 44px 导航栏遮挡 - 35px 标题区 ≈ 580px（1.1rem 标题方案，标题区更紧凑）。生成前必须估算每页总高度
- **封面 CSS 必须 `position: absolute`**（不是 relative），否则无法铺满 slide
- **⚠️ 深色背景文字防御（必读）**：`.reveal h1/h2/h3` 全局颜色会覆盖封面、深色卡片（suggest-card、big-data-card）内的文字颜色。必须在 CSS 中加入 `[data-state="cover"] h1, [data-state="cover"] p { color: #fff !important; }` 和 `.suggest-card h3 { color: #fff !important; }` 等防御规则。详见 html-template.md
- **⚠️ 垂直导航 API（必读）**：所有slide在同一个外层 `<section>` 的垂直栈中，导航跳转必须用 `Reveal.slide(0, indexV)`，第一个参数固定为0。`slidechanged` 事件中用 `event.indexv`（不是 `event.indexh`）判断当前页。详见 html-template.md
- **禁止 localStorage 自动恢复**：编辑保存功能保留，但绝不自动从 localStorage 加载旧内容覆盖新生成的 HTML
- **CDN 源必须使用 cdnjs.cloudflare.com**：jsdelivr.net 在中国大陆不可访问，导致 Font Awesome 图标显示为色块、Reveal.js 无法加载。所有外部资源（reveal.js、font-awesome、echarts）统一使用 `cdnjs.cloudflare.com`
- **页面入场动画（标配）**：内容页用 CSS 预隐藏子元素（`opacity:0`）+ JS 带 transition 显示；封面页用 `@keyframes` + `.cover-el` 类自动播放。代码已在 html-template.md 中，直接包含即可
- **叙事动画（必须使用，基于 Reveal.js fragment）**：关键页面的元素使用 fragment 实现点击逐步出现，拒绝均匀节奏：
  - **`class="fragment"`** — 默认 fragment，点击后淡入上移出现。用于数据卡片、结论条目等
  - **`class="fragment impact"`** — 焦点数字弹入，scale从0.85弹到1.0，带过冲缓动。用于71.7%、$1M等核心数据
  - 不加 fragment 的元素 — 随页面切换自动入场（默认stagger淡入），和之前一样
  - **Reveal.js fragment 原生行为**：点击/方向键/空格逐步显示 fragment 元素，全部显示后才翻页。不需要任何自定义 JS
  - **使用频率**：10页PPT中约2-3页需要 fragment。不是每页都要，但关键数据页和结论页必须有
  - **焦点数字的HTML模式**：焦点数字不应缩在卡片里，应该独立放大展示：
    ```html
    <!-- ❌ 焦点数据缩在卡片里，和普通信息一样 -->
    <div class="card"><div class="data-big">71.7%</div><div class="data-label">解决率</div></div>
    <!-- ✅ 焦点数据独立放大，fragment impact 弹入 -->
    <div class="fragment impact" style="flex:1.2;background:...;padding:20px;...">
      <div style="font-size:3.2rem;font-weight:200;color:var(--accent);line-height:1">71.7<small style="font-size:0.4em;opacity:0.6">%</small></div>
      <div style="font-size:0.85rem;margin-top:6px">SWE-bench Verified 最高解决率</div>
    </div>
    <!-- 右侧卡片也逐步出现 -->
    <div class="card hl fragment">缺陷检测</div>
    <div class="card hl fragment">智能辅导</div>
    ```
- **封面内容元素**：必须添加 `class="cover-el"` 以支持封面入场动画
- **悬停高亮**：内容模块（卡片、数据块、功能块、渐变色块）必须添加 `class="hl"` 以支持鼠标悬停三层效果：
  - **浮起**：`transform: translateY(-4px) scale(1.02)` — 卡片微上浮+微放大
  - **边框强调**：`border-color` 从淡色 → 主题色（`!important` 覆盖内联样式），无光晕时仍有强调感
  - **光晕**：`box-shadow: 0 0 15px rgba(var(--accent-rgb), 0.3), 0 0 30px rgba(var(--accent-rgb), 0.15)` — 双层主题色光晕
  - **主题感知**：深色背景页（`.slide-dark`）= 主题色双层光晕；浅色背景页 = 暗色投影阴影。两者都有主题色边框强调
  - **多主题色卡片**：非主色卡片用 `[style*="border-color:rgba(X,Y,Z"]` 选择器精确匹配，悬停时显示对应颜色光晕（如暗红色卡片 → 暗红色光晕）
- **双色交错配色**：暗色主题下，同层级多项卡片（≥3张）采用主色+辅助色交错排列，避免同色相邻：
  - 主题变量需定义 `--accent2` / `--accent2-rgb`（辅助色）
  - 奇数位卡片用**字面值** `border-color:rgba(R,G,B,0.2)`（R,G,B 来自 `--accent-rgb`）+ `var(--accent)` 强调元素
  - 偶数位卡片用**字面值** `border-color:rgba(R,G,B,0.2)`（R,G,B 来自 `--accent2-rgb`）+ `var(--accent2)` 强调元素
  - ⚠️ 内联 `border-color` 必须写字面 RGB 值（如 `rgba(255,0,170,0.2)`），不能用 `var(--accent-rgb)` — CSS `style*` 选择器只匹配字面值
  - 配套悬停效果：主色卡片 → 主色光晕，辅助色卡片 → 辅助色光晕（boilerplate.html 中已有 `style*` 选择器匹配字面值）
- **画笔工具**：始终包含（参考 html-template.md 中的实现），右下角浮动按钮 + D键切换
- **演讲者模式**：始终包含（参考下方实现），右下角浮动按钮（显示器图标）+ S键切换
  - 每页 slide 内必须包含 `<div class="speaker-notes" style="display:none">讲稿内容</div>`
  - 备注内容从资料中提取——放不进 slide 但演讲时需要说的内容
  - 按 S 弹出独立窗口，显示：当前页内容摘要、下一页预览、可编辑备注、计时器、页码
  - 备注支持实时编辑，修改后同步回主文档对应页的 speaker-notes div
- **无障碍动画降级**：CSS 中必须包含 `@media (prefers-reduced-motion: reduce)` 规则，将所有动画时长降为 0.01ms 或直接跳过。尊重用户的系统级减少动画设置
  ```css
  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
  }
  ```

#### 6.3 生成策略（基于骨架文件）

**核心：读取 [boilerplate.html](boilerplate.html) 骨架文件，替换占位符，不要从零写。**

**步骤：**

**1. 读取骨架文件 `boilerplate.html`**

**2. 一次性替换以下占位符：**
- `{{TITLE}}` — HTML 标题
- `{{BRAND}}` — 导航栏品牌名
- `{{NAV_LINKS}}` — 导航链接（`<a href="#" data-slide="1">章节</a>`）
- `{{SLIDE_CONTENT}}` — 所有页面 HTML（封面 + P2-PN）
- `:root {}` 内的主题变量 — 替换为选定主题的颜色/字体

**3. 主题变量替换规则：**
- 只改 `:root {}` 里的变量值，不改其他 CSS
- 封面布局在骨架中已预定义三种（C-1/C-2/C-3），根据用户选择使用对应的 HTML 结构
- C-3 Hero 全出血封面需要额外 CSS：背景层(`position:absolute` + 全出血渐变/图片)、遮罩层(半透明)、内容层(白色文字 + `!important` 防御主题覆盖)

**3b. 用户图片嵌入规则（阶段1选了"有本地图片"时）：**
- 用户提供本地图片时，用 Bash 将图片转为 base64 内联到 HTML，保持单文件输出：
  ```bash
  python -c "import sys,base64; sys.stdout.reconfigure(encoding='utf-8'); data=base64.b64encode(open('图片路径','rb').read()).decode(); print(f'data:image/jpeg;base64,{data}')"
  ```
- **支持格式：** JPG/JPEG、PNG、WebP、GIF（静态）、BMP、SVG
  - SVG 直接用 Read 读取文本内容内联，不需要 base64
  - 其他格式按文件扩展名自动匹配 MIME 类型：jpg→image/jpeg, png→image/png, webp→image/webp, gif→image/gif, bmp→image/bmp
  - 优先推荐 **WebP**（同画质体积最小）和 **JPG**（通用性最好），PNG 适合需要透明通道的图片
- 图片用于 C-3/H-1 全出血背景时，写入 `.hero-bg` 的 `background-image`：
  ```html
  <div class="hero-bg" style="background-image: url('data:image/jpeg;base64,...'); background-size: cover; background-position: center;"></div>
  ```
- 图片用于 H-2 左右交替图文时，用 `<img src="data:image/...">` 内联
- **单张图片不超过 500KB**（base64 后约 670KB），超过时提示用户压缩
- 未提供图片的页面仍用 CSS 渐变/SVG 生成背景
- 深色主题（如 Data Dark、Gradient Fashion）需改 `html{background}` 和 `top-nav` 样式

**4. 卡片 HTML 模板（直接复制使用）：**

图标+数据卡片：
```html
<div class="card hl" style="display:flex;gap:12px;align-items:flex-start">
  <div class="card-icon"><i class="fas fa-XXX"></i></div>
  <div><div class="data-big">72<small>亩</small></div><div class="data-label">用地面积</div></div>
</div>
```

图标+标题+正文卡片：
```html
<div class="card hl" style="display:flex;gap:12px;align-items:flex-start">
  <div class="card-icon"><i class="fas fa-XXX"></i></div>
  <div><div class="card-title">标题</div><div class="card-text">描述文字</div></div>
</div>
```

纯数据指标（深色页）：
```html
<div class="card hl"><div class="metric-val">15%</div><div class="metric-desc">企业所得税率</div></div>
```

**5. 适用规则：**
- 所有页数：读骨架 → 替换占位符 → Write 完整文件
- 如页面超过 20 页，Write 后检查是否有截断，如有则 Edit 补充

**6. 大文件输出策略（防止截断）：**
- **15页以上PPT必须分3步生成**，禁止一次性全量输出：
  1. **先写骨架** — CSS/JS框架 + 主题变量 + 封面页
  2. **填P2-P9** — 第一章 + 第二章前半
  3. **填P10-P15+** — 第二章后半 + 第三章
- **大文件用 Python 脚本写入** — 先写 .py 文件再执行，**禁止在 Bash heredoc 里塞大段 HTML**（转义死循环风险极高）
- 15页以下PPT可一次性 Write 完整文件

#### 上下文管理建议（防止生成质量下降）

**问题：** 20页以上PPT在骨架+填充过程中，上下文窗口接近上限时生成质量会明显下降（遗漏属性、布局重复、CSS冲突）。

**建议：**
- 21-30页PPT优先使用**一次性 Write 完整文件**策略，避免多轮 Edit 消耗上下文
- 如果必须分批（骨架+填充），在 Write 骨架后、Edit 填充前，**建议用户使用 `/clear` 清空对话**，然后重新读取骨架文件再填充
- 生成过程中如果发现开始遗漏细节（忘记 `hl` class、漏掉封面动画等），这是上下文过载的信号，应切换为一次性 Write 策略

**输出文件：** 在工作目录生成 `{主题名称}.html`，立即用 `start "" "文件路径"` 自动打开（核心原则第6条）。

**生成完成后，询问用户是否需要进一步操作：**

用 AskUserQuestion 提供4个选项：
- **部署到URL** — `bash scripts/deploy.sh <文件>`，生成在线访问链接
- **导出PDF** — `bash scripts/export-pdf.sh <文件>`
- **导出离线版本** — 将CDN资源内联为完全离线的HTML（见下方说明）
- **不需要** — 直接交付当前文件

**离线版本导出流程（当用户选择"导出离线版本"时）：**

**PDF 压缩选项：** 导出 PDF 后如果文件超过 10MB，主动询问用户是否压缩。使用 `--compact` 参数渲染 1280×720（而非 1920×1080），文件体积减少 50-70%：
```bash
bash scripts/export-pdf.sh <文件> output.pdf --compact
```

⚠️ 离线版本适用于：无网络环境的会议室、内网环境、需提前下载备用。

生成步骤：
1. 复制原HTML为 `{主题名称}-offline.html`
2. 逐个下载CDN资源并内联替换：

```bash
# 下载 Reveal.js CSS → 替换 <link> 为 <style>
curl -s "https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.6.1/reveal.min.css" -o /tmp/reveal.min.css
curl -s "https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.6.1/theme/white.min.css" -o /tmp/white.min.css

# 下载 Reveal.js JS → 替换 <script src> 为 <script>内容</script>
curl -s "https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.6.1/reveal.min.js" -o /tmp/reveal.min.js

# 下载 Font Awesome CSS + 字体文件
curl -s "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css" -o /tmp/fa.min.css

# 下载 ECharts（仅数据驱动/平衡型包含时）
curl -s "https://cdnjs.cloudflare.com/ajax/libs/echarts/5.5.0/echarts.min.js" -o /tmp/echarts.min.js

# 下载 Google Fonts CSS
curl -s "https://fonts.googleapis.com/css2?family=XXX&display=swap" -o /tmp/fonts.css
# 字体文件也需要下载并 base64 内联
```

3. 用 Python 脚本完成内联替换：

```python
python -c "
import sys, re
sys.stdout.reconfigure(encoding='utf-8')
html = open(r'{文件路径}', 'r', encoding='utf-8').read()

# 替换 <link rel='stylesheet' href='CDN_URL'> → <style>CSS内容</style>
# 替换 <script src='CDN_URL'></script> → <script>JS内容</script>
# Font Awesome: CSS中 @font-face 的 url() 替换为 base64 data URI

# ... 具体替换逻辑
open(r'{输出路径}', 'w', encoding='utf-8').write(html)
"
```

4. 输出离线文件，告知用户文件体积变化（约增大 1-2MB）

**注意事项：**
- Font Awesome 字体文件（woff2）需 base64 内联到 CSS 中，这是体积增大的主要来源
- Google Fonts 如果无法下载（被墙），回退到系统字体即可，不影响功能
- 离线版本与在线版本功能完全一致，只是体积更大
- 建议优先用在线版本，离线版本作为备用

---

## ⚠️ 迭代更新规则（防止回归）

当用户对已生成的PPT提出修改需求（加功能、改页面、调风格）时，必须遵守以下规则，防止"改了A功能，B功能坏了"。

### 原则1：先备份再改动

每次修改前，先创建备份目录并存入备份：
```bash
mkdir -p "D:/CLAUDE/PPT制作/.backups"
cp "文件.html" "D:/CLAUDE/PPT制作/.backups/文件_$(date +%Y%m%d_%H%M%S).html"
```

**备份后必须告诉用户：**
> 备份已保存到 `D:/CLAUDE/PPT制作/.backups/文件_20260405_143022.html`，如果改坏了可以随时恢复。

**恢复方式：**
```bash
cp "D:/CLAUDE/PPT制作/.backups/备份文件名.html" "原文件.html"
```

**多步编辑中间备份：** 每完成2-3个Edit，更新一次备份点（执行同样的cp命令）。这样后续Edit出错时恢复到最近的中 checkpoints，而不是最初的备份，避免丢失已完成的改动。

### 原则2：外科手术式 Edit — 三级升级路径

**禁区：** 整段重写整个 `<style>` 或 `<script>` 块（极易误删已有功能）

**L1 行级 Edit（默认）**
直接 Edit，old_string 在文件中唯一。
- CSS 新增：在 `<style>` 块末尾（`</style>` 前）插入新规则，不修改已有规则
- JS 新增：在对应功能模块注释后插入，不修改已有代码
- HTML 修改：精确匹配目标页面的 `<!-- 📝 第X页 -->` 注释，只替换该页内容
- 配置修改（如 Reveal.initialize 参数）：精确匹配当前配置行替换

**L2 扩大上下文（L1失败时）**
Edit 报 "old_string is not unique" → 先用 Grep 确认匹配数量，然后加前后行扩大 old_string 范围重试。仍然只改目标内容，只是匹配范围更大。

**L3 模块级替换（L2仍失败时）**
用注释边界定位整个功能模块，替换该模块全部内容。模块边界示例：
- CSS: `/* === 画笔 === */` … `/* === 编辑 === */`
- JS: `// ========== 画笔 ==========` … `// ========== 编辑 ==========`
- HTML: `<!-- 📝 第3页：XXX -->` … `<!-- 📝 第4页：XXX -->`

L3 强制规则（必须全部执行）：
1. 替换前 Read 当前模块完整内容（不凭记忆写，从文件读取）
2. 替换后 Read 被替换区域，确认没丢规则
3. 跑原则3的 Grep 回归验证
4. 通知用户检查被替换模块的功能

**升级判断：** 能用 L1/L2 就不升到 L3。L3 是升级手段，不是首选。

### 原则3：改后回归验证清单

每次修改完成后，必须用 Grep 验证核心功能（不是手动检查，用工具确认）。关键项查核心代码片段，不只查名字：

```
回归验证项（每次修改后必检）：
□ Reveal.js 配置         — grep "mouseWheel: true" 确认完整配置
□ 垂直导航 API           — grep "Reveal.slide(0," 确认第一个参数为0
□ 导航高亮               — grep "event.indexv"（不是 indexh）
□ 封面动画双轨           — grep "function animateCoverSlide" + grep "\.ce"
□ 内容入场动画           — grep "function animateContentSlide" + grep "translateY(0)"
□ 画笔工具               — grep "penCanvas.classList.toggle" 确认参数完整
□ 编辑功能               — grep "function toggleEdit" + grep "contentEditable"
□ 演讲者模式             — grep "function toggleSpeaker" + grep "speakerWin"
□ 每页备注               — grep "speaker-notes" 确认每页有备注
□ 封面白色文字防御       — grep 'data-state="cover"].*color.*#fff'
□ 深色卡片文字防御       — grep 'suggest-card h3.*color.*#fff'
□ 悬停高亮               — grep '\.hl:hover' 确认效果完整
□ Fragment 叙事动画       — grep "fragment.impact" + grep "fragment.visible"
```

如果验证失败，立即从备份恢复，然后重新做更小范围的修改。

### 原则4：功能模块边界清晰

HTML 文件中的功能模块通过注释严格分隔，修改时以注释为边界：

```html
<!-- CSS 区域 -->
<style>
  /* === 主题变量 === */
  /* === 基础样式 === */
  /* === 导航栏 === */
  /* === 封面样式 === */
  /* === 卡片布局 === */
  /* === 图表容器 === */
  /* === 动画 === */
  /* === 画笔 === */
  /* === 编辑 === */
  /* === 用户新增功能 === */    ← 新功能加在这里
</style>

<!-- JS 区域 -->
<script>
  // ========== Reveal.js 初始化 ==========
  // ========== 导航栏 ==========
  // ========== 图表 ==========
  // ========== 入场动画 ==========
  // ========== 封面动画 ==========
  // ========== 编辑 ==========
  // ========== 画笔 ==========
  // ========== 演讲者模式 ==========
  // ========== 用户新增功能 ==========    ← 新功能加在这里
</script>
```

### 回滚策略

如果用户要求"去掉刚才加的功能"：
- 优先从备份恢复（最快、最安全）
- 如果没有备份，精确删除新增的代码块（依赖注释边界定位）
- **绝不通过重写整个文件来回滚**

---

## 代码注释规范

- `<!-- 📝 第X页：{标题} — 以下是页面内容，可直接编辑 -->` — 每个section前
- `<div class="speaker-notes" style="display:none">讲稿内容</div>` — 每个section末尾（`</section>` 前）
- `<!-- 📊 以下是图表数据，可修改 -->` — 图表相关代码前
- `<!-- 📌 固定顶部导航栏 -->` — 导航栏区域
- `// 📊 以下是图表数据，可修改` — 图表配置
- `// ========== 功能模块名 ==========` — JS模块分隔

---

## 支持文件

| 文件 | 用途 | 何时读取 |
|------|------|----------|
| [boilerplate.html](boilerplate.html) | **即用型骨架文件（CSS/JS/结构完整）** | **阶段6（必须，第一个读）** |
| [STYLE_PRESETS.md](STYLE_PRESETS.md) | 19套视觉预设含6套品牌风格（颜色/字体/质感） | 阶段5 |
| [LAYOUT_PATTERNS.md](LAYOUT_PATTERNS.md) | 布局模式库（33种页面结构模板） | **阶段6（必须）** |
| [html-template.md](html-template.md) | HTML架构参考（技术要点文档） | 阶段6（可选） |
| [animation-patterns.md](animation-patterns.md) | 封面动画、背景效果、入场动画 | 阶段6（可选） |
| [scripts/deploy.sh](scripts/deploy.sh) | 部署到Vercel | 阶段6后（可选） |
| [scripts/export-pdf.sh](scripts/export-pdf.sh) | 导出为PDF | 阶段6后（可选） |

---

## 致谢

> 阶段3「内容诊断」模块的规划思路参考了 [女娲·Skill造人术](https://github.com/alchaincyf/nuwa-skill) 的 Steve Jobs 视角 Skill（MIT License）
