<!--
  Slides Skill — 主题预设参考
  MIT License — © 2025 Zara Zhang (original framework)
                © 2026 Hongqing W (enhanced version)
  https://github.com/zarazhangrui/frontend-slides
-->
# 主题预设参考

19套精心设计的视觉预设（含6套品牌风格 + 1套中国风），每套都有独特的字体、配色和签名元素。
**仅使用抽象几何形状 — 不使用插画。**

**响应式基础：** 所有预设必须配合 [viewport-base.css](viewport-base.css) 使用。

---

## 暗色主题

### 1. Bold Signal（醒目信号）

**气质：** 自信、大胆、高冲击力
**适用场景：** 产品发布、融资路演、重大宣布

**字体：**
- 标题：`Archivo Black` (900)
- 正文：`Space Grotesk` (400/500)
- 中文回退：`'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', sans-serif`

**配色：**
```css
:root {
    --bg-primary: #1a1a1a;
    --bg-gradient: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 50%, #1a1a1a 100%);
    --card-bg: #FF5722;
    --text-primary: #ffffff;
    --text-on-card: #1a1a1a;
    --accent: #FF5722;
}
```

**签名元素：**
- 大胆的彩色卡片作为焦点（橙/珊瑚色调）
- 大号章节编号（01, 02...）
- 导航面包屑带活跃/非活跃透明度

**组件样式：**

*按钮：*
- 主按钮：bg `#FF5722`，白字（`#ffffff`），圆角 8px，`font-weight: 700`（Archivo Black），hover 时 `filter: brightness(1.15)` + `transform: translateY(-2px)`
- 次按钮：bg transparent，`#FF5722` 文字，`2px solid #FF5722`，圆角 8px，hover 时填充 `bg rgba(255,87,34,0.1)`
- 强调按钮：bg `#1a1a1a`，`#FF5722` 文字，`1px solid #FF5722`，圆角 8px，hover 时 `border-color: #ffffff`

*卡片：*
- 普通卡片：bg `#2d2d2d`，圆角 8px，`border: none`，`box-shadow: 0 8px 32px rgba(0,0,0,0.4)`
- 高亮/焦点卡片：bg `#FF5722`（橙/珊瑚色块），文字 `#1a1a1a`，圆角 8px，`box-shadow: 0 12px 40px rgba(255,87,34,0.3)`，标题用 Archivo Black
- 数据卡片：bg `#1a1a1a`，`2px solid #FF5722` 顶部边框，圆角 8px，数据数字用 `#FF5722` + `font-size: 3rem` + Archivo Black

*标签/徽章：*
- 方角（`border-radius: 0` 或 2px），bg `#FF5722`，白字，`font-weight: 700`（Space Grotesk 500），`text-transform: uppercase`
- 章节编号标签：超小字号，`#FF5722` 文字，`letter-spacing: 2px`，如 "01 / 05"
- 暗色标签：bg `#2d2d2d`，`#FF5722` 文字，`1px solid #FF5722`

*阴影层级：*
- L1 卡片：`0 4px 16px rgba(0,0,0,0.3)` — 深色基底上的微妙浮起
- L2 重阴影：`0 8px 32px rgba(0,0,0,0.5)` — 普通卡片和交互元素
- L3 强调阴影：`0 12px 48px rgba(255,87,34,0.25), 0 4px 16px rgba(0,0,0,0.4)` — 橙色色块焦点卡片，带暖色光晕

*装饰线：*
- 分割线：`2px solid #FF5722`（粗线，自信），长度不超过容器 40%，左对齐
- 面包屑分隔：`color: rgba(255,255,255,0.3)`，活跃项 `color: #FF5722`

**设计规则：**

*推荐：*
- 橙色(#FF5722)大色块作为焦点，每页最多2处橙色高亮
- 使用超大章节编号(01, 02...)建立页面间节奏感
- 阴影带橙色光晕时才使用L3级别，日常停留在L1-L2

*避免：*
- 不要将 #FF5722 同时用于标题、背景和装饰
- 不要使用细线或轻量圆角 — border-radius 不超过 8px
- 不要添加渐变背景 — 纯色深灰才是正确基调

---

### 2. Electric Studio（电力工作室）

**气质：** 大胆、干净、专业、高对比
**适用场景：** 技术分享、B2B汇报、企业介绍

**字体：**
- 标题：`Manrope` (800)
- 正文：`Manrope` (400/500)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-dark: #0a0a0a;
    --bg-white: #ffffff;
    --accent-blue: #4361ee;
    --text-dark: #0a0a0a;
    --text-light: #ffffff;
}
```

**签名元素：**
- 双面板垂直分割（上白下蓝）
- 面板边缘的强调色条
- 引用排版作为主视觉元素

**组件样式：**

*按钮：*
- 主按钮：bg `#4361ee`（蓝），文字 `#ffffff`，圆角 4px，hover 时 `filter: brightness(1.15)` + `box-shadow: 0 4px 12px rgba(67,97,238,0.3)`
- 反转按钮：bg `#ffffff`，文字 `#0a0a0a`，圆角 4px，`1px solid #e0e0e0`，hover 时 bg `#0a0a0a` + 文字变白
- 强调按钮：bg `#0a0a0a`（纯黑），文字 `#4361ee`，圆角 4px，`border: 1px solid #4361ee`

*卡片：*
- 普通卡片（白底）：bg `#ffffff`，圆角 6px，`border: 1px solid #e0e0e0`，`box-shadow: 0 2px 8px rgba(0,0,0,0.06)`
- 普通卡片（暗底）：bg `#0a0a0a`，圆角 6px，`border: 1px solid #1a1a1a`
- 双色卡片：上半 bg `#ffffff` + 下半 bg `#4361ee`（呼应双面板签名元素），用 `background: linear-gradient(180deg, #ffffff 50%, #4361ee 50%)`
- 引用卡片：bg `#ffffff`，左侧 `border-left: 4px solid #4361ee`，大号引用文字用 Manrope 800 + `font-style: italic`

*标签/徽章：*
- 圆角 4px，bg `rgba(67,97,238,0.08)`，文字 `#4361ee`，字体 Manrope 600
- 色条标签：`border-left: 3px solid #4361ee`，用于面板边缘标记
- 对比标签：左侧黑底白字 `#0a0a0a` + 右侧蓝底白字 `#4361ee`，拼接使用

*阴影层级：*
- L1 卡片：`0 2px 8px rgba(0,0,0,0.06)` — 干净轻微
- L2 蓝阴影：`0 4px 16px rgba(67,97,238,0.12)` — 蓝色调悬停
- L3 分割层：`0 8px 32px rgba(0,0,0,0.12)` — 深度分层，用于模态/弹出

*装饰线：*
- 分割线：`2px solid #4361ee`（粗线，专业），用于双面板分割
- 面板边缘色条：`width: 4px; background: #4361ee` 垂直色条装饰

**设计规则：**

*推荐：*
- 利用双面板分割(上白下蓝)创造页面间的节奏变化
- 引用卡片配合 Manrope 800 + italic 打造权威感
- 面板边缘色条是核心签名元素，每页至少出现一处

*避免：*
- 不要使用超过3种颜色 — 白/黑/蓝已足够
- 不要给卡片加圆角超过 6px — 本主题追求专业精准
- 不要让蓝色大面积铺底 — 蓝色是功能色，不是装饰色

---

### 3. Neon Cyber（霓虹赛博）

**气质：** 未来感、科技感、自信
**适用场景：** AI/科技产品、黑客马拉松、前沿技术

**字体：**
- 标题：`Clash Display` (Fontshare)
- 正文：`Satoshi` (Fontshare)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-primary: #0a0f1c;
    --accent-cyan: #00ffcc;
    --accent-magenta: #ff00aa;
    --text-primary: #ffffff;
    --text-secondary: #64748b;
}
```

**签名元素：**
- 粒子背景（Canvas）— 实现代码见 animation-patterns.md「粒子背景」章节
- 霓虹发光效果
- 网格图案叠加
- 青色+品红双强调

**组件样式：**

*按钮：*
- 主按钮：bg `rgba(0,255,204,0.15)`，文字 `#00ffcc`，`1px solid rgba(0,255,204,0.4)`，圆角 6px，hover 时 `box-shadow: 0 0 16px rgba(0,255,204,0.3)` 发光扩散
- 强调按钮：bg `#ff00aa`，白字，圆角 6px，hover 时 `box-shadow: 0 0 20px rgba(255,0,170,0.4)` 品红发光
- 幽灵按钮：bg transparent，`#00ffcc` 文字 + `1px solid rgba(0,255,204,0.2)`，hover 时边框变实

*卡片：*
- 普通卡片：bg `rgba(15,23,42,0.6)`（半透明深蓝），圆角 8px，`border: 1px solid rgba(0,255,204,0.12)`，`backdrop-filter: blur(8px)`
- 高亮卡片：bg `rgba(0,255,204,0.08)`，`border: 1px solid rgba(0,255,204,0.3)`，`box-shadow: 0 0 24px rgba(0,255,204,0.1)` 微弱青色辉光
- 数据卡片：bg `rgba(255,0,170,0.06)`，`border: 1px solid rgba(255,0,170,0.2)`，数据数字用 `#00ffcc` 超大字号

*标签/徽章：*
- 圆角 4px（略方，科技感），bg `rgba(0,255,204,0.1)`，文字 `#00ffcc`，字体 Satoshi 500
- 品红标签：bg `rgba(255,0,170,0.1)`，文字 `#ff00aa`
- 章节标签：可在左侧加 `border-left: 2px solid #00ffcc` 竖线装饰

*阴影层级：*
- L1 卡片：`0 4px 20px rgba(0,0,0,0.4)` — 深色基底上的微妙浮起
- L2 发光：`0 0 20px rgba(0,255,204,0.15)` — 青色光晕，用于 hover / 高亮元素
- L3 强发光：`0 0 30px rgba(0,255,204,0.25), 0 0 60px rgba(255,0,170,0.1)` — 双色辉光，仅用于封面焦点元素

*装饰线：*
- 分割线：`1px solid rgba(0,255,204,0.15)`，两端渐隐（`mask-image: linear-gradient(...)`）
- 网格背景：`background-image: linear-gradient(rgba(0,255,204,0.03) 1px, transparent 1px)` 重复网格

**设计规则：**

*推荐：*
- 青色(#00ffcc)和品红(#ff00aa)保持严格的功能分工：青色=信息/数据，品红=强调/行动
- 半透明+blur 是核心语言，卡片和按钮都应遵循
- 网格背景是签名元素，但透明度控制在 0.03 以内

*避免：*
- 不要同时让青色和品红在同一元素上发光 — 选一个主色
- 不要使用不透明背景 — 所有容器都应有 backdrop-filter: blur
- 不要引入第三种鲜艳色 — 双色系统是限制也是力量

---

### 4. Dark Botanical（暗夜植物）

**气质：** 优雅、精致、高级感
**适用场景：** 奢侈品、文化艺术、高端品牌

**字体：**
- 标题：`Cormorant` (400/600) — 优雅衬线
- 正文：`IBM Plex Sans` (300/400)
- 中文回退：`'Noto Serif SC', 'Source Han Serif', serif`

**配色：**
```css
:root {
    --bg-primary: #0f0f0f;
    --text-primary: #e8e4df;
    --text-secondary: #9a9590;
    --accent-warm: #d4a574;
    --accent-pink: #e8b4b8;
    --accent-gold: #c9b896;
}
```

**签名元素：**
- 抽象柔和渐变圆（模糊、重叠）
- 暖色调点缀（粉、金、赭）
- 细竖线装饰
- 斜体签名排版

**组件样式：**

*按钮：*
- 主按钮：bg `rgba(212,165,116,0.12)`，文字 `#d4a574`，圆角 16px，`1px solid rgba(212,165,116,0.25)`，hover 时 `box-shadow: 0 0 20px rgba(212,165,116,0.15)`
- 强调按钮：bg `#d4a574`，文字 `#0f0f0f`，圆角 16px，hover 时 `filter: brightness(1.1)`
- 幽灵按钮：bg transparent，文字 `#e8b4b8`，`1px solid rgba(232,180,184,0.2)`，圆角 16px

*卡片：*
- 普通卡片：bg `rgba(255,255,255,0.03)`，圆角 12px，`border: 1px solid rgba(212,165,116,0.08)`，`box-shadow: 0 8px 32px rgba(212,165,116,0.06)`
- 高亮卡片：bg `rgba(212,165,116,0.06)`，`border: 1px solid rgba(212,165,116,0.2)`，`box-shadow: 0 0 24px rgba(212,165,116,0.1)` 暖金辉光
- 数据卡片：bg `rgba(232,180,184,0.05)`，`border: 1px solid rgba(232,180,184,0.15)`，数据数字用 `#d4a574` 大字号

*标签/徽章：*
- 圆角 12px（柔和），bg `rgba(212,165,116,0.1)`，文字 `#d4a574`，字体 IBM Plex Sans 500
- 粉色标签：bg `rgba(232,180,184,0.1)`，文字 `#e8b4b8`
- 章节标签：左侧 `border-left: 2px solid #c9b896` 细竖线装饰，文字用斜体

*阴影层级：*
- L1 卡片：`0 6px 24px rgba(0,0,0,0.3)` — 暗底上的柔和浮起
- L2 暖辉光：`0 0 20px rgba(212,165,116,0.1)` — 暖金光晕，hover 元素
- L3 双暖辉光：`0 0 30px rgba(212,165,116,0.15), 0 0 60px rgba(232,180,184,0.08)` — 金粉双色辉光，封面焦点

*装饰线：*
- 分割线：`1px solid rgba(212,165,116,0.12)`，两端渐隐
- 竖线装饰：`width: 1px; background: linear-gradient(180deg, #d4a574, transparent)` 用于章节分隔

**设计规则：**

*推荐：*
- 暖金(#d4a574)是视觉锚点，用于标题、数据和关键交互元素
- 装饰线使用渐隐效果(两端渐隐)，营造优雅感
- 圆角保持 12-16px 的柔和范围，与衬线字体气质一致

*避免：*
- 不要使用纯白(#ffffff)文字 — 使用 #e8e4df 暖白
- 不要让粉色和金色同等权重 — 金色主导，粉色点缀
- 不要使用粗线或直角 — 本主题追求柔软精致

---

## 亮色主题

### 5. Swiss Modern（瑞士现代）

**气质：** 干净、精准、包豪斯风格
**适用场景：** 设计展示、数据报告、极简风格

**字体：**
- 标题：`Archivo` (800)
- 正文：`Nunito` (400)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-primary: #ffffff;
    --text-primary: #000000;
    --accent: #ff3300;
    --text-secondary: #666666;
}
```

**签名元素：**
- 可见网格线
- 不对称布局
- 几何形状装饰
- 红色单一强调色

**组件样式：**

*按钮：*
- 主按钮：bg `#ff3300`，文字 `#ffffff`，圆角 2px（近乎直角，包豪斯），`font-weight: 800`（Archivo），hover 时 `transform: translateX(4px)` 几何位移
- 次按钮：bg transparent，文字 `#000000`，`2px solid #000000`，圆角 0，hover 时 bg `#000` + 文字变白
- 线框按钮：bg transparent，文字 `#ff3300`，`1px solid #ff3300`，圆角 0，hover 时填充红色

*卡片：*
- 普通卡片：bg `#ffffff`，圆角 0，`border: 1px solid #e0e0e0`，无阴影，严格网格对齐
- 高亮卡片：bg `#ffffff`，`border-left: 4px solid #ff3300`（红色左边线强调），其余三边 `1px solid #e0e0e0`
- 数据卡片：bg `#ffffff`，`border: 1px solid #000000`，数据数字用 `#ff3300` + `font-size: 2.5rem` + Archivo 800

*标签/徽章：*
- 直角（border-radius: 0），bg `#ff3300`，白字，`font-weight: 800`，`text-transform: uppercase`，`letter-spacing: 1px`
- 中性标签：bg `#f0f0f0`，文字 `#666666`，直角
- 网格标签：背景带网格线纹理 `background-image: linear-gradient(#e0e0e0 1px, transparent 1px)`

*阴影层级：*
- L1 卡片：无阴影 — 纯边框，包豪斯不依赖阴影
- L2 微浮：`0 2px 8px rgba(0,0,0,0.06)` — 仅用于弹出层/模态
- L3 结构层：`0 4px 16px rgba(0,0,0,0.08)` — 仅用于叠放的纸张效果

*装饰线：*
- 分割线：`1px solid #000000`（纯黑，精确），或 `1px solid #ff3300`（红色强调）
- 网格线：`1px solid #f0f0f0` 贯穿页面，形成可见的网格结构

**设计规则：**

*推荐：*
- 红色(#ff3300)作为唯一强调色，用于关键标注和行动元素
- 网格线是签名元素，使用极淡的 #f0f0f0 贯穿页面
- 不对称布局是核心 — 避免居中对称

*避免：*
- 不要使用阴影 — 包豪斯不依赖阴影，纯边框分层
- 不要使用圆角 — border-radius 保持 0-2px
- 不要添加装饰图形 — 只用网格和色块

---

### 6. Notebook Tabs（笔记本标签）

**气质：** 编辑风格、有条理、优雅触感
**适用场景：** 教学课件、研究报告、知识分享

**字体：**
- 标题：`Bodoni Moda` (400/700) — 经典编辑
- 正文：`DM Sans` (400/500)
- 中文回退：`'Noto Serif SC', 'Source Han Serif', serif`

**配色：**
```css
:root {
    --bg-outer: #2d2d2d;
    --bg-page: #f8f6f1;
    --text-primary: #1a1a1a;
    --tab-1: #98d4bb;
    --tab-2: #c7b8ea;
    --tab-3: #f4b8c5;
    --tab-4: #a8d8ea;
    --tab-5: #ffe6a7;
}
```

**签名元素：**
- 纸张容器 + 柔和阴影
- 右侧彩色章节标签（竖排文字）
- 左侧装订孔装饰
- 标签字号随视口缩放

**组件样式：**

*按钮：*
- 主按钮：bg `#f8f6f1`（纸张色），文字 `#1a1a1a`，圆角 0，`1px solid #d4d0c8`，`box-shadow: 2px 2px 0 #d4d0c8`（纸张层叠感），hover 时阴影位移 `4px 4px`
- 彩色按钮：bg `#98d4bb`（标签色），文字 `#1a1a1a`，圆角 0，hover 时切换为 `#c7b8ea`
- 线框按钮：bg transparent，文字 `#1a1a1a`，`1px dashed #9a9590`（虚线，像手写笔记边框）

*卡片：*
- 普通卡片：bg `#f8f6f1`，圆角 0，`box-shadow: 3px 3px 0 #d4d0c8`（纸张叠放效果），`border-left: 3px double #9a9590`（双线装饰像笔记本边距）
- 高亮卡片：bg `#98d4bb`（薄荷标签色），文字 `#1a1a1a`，圆角 0，`box-shadow: 4px 4px 0 rgba(0,0,0,0.1)`
- 数据卡片：bg `#f8f6f1`，右上角折角效果 `box-shadow`，数据数字用 Bodoni Moda 700

*标签/徽章：*
- 竖排标签：宽 24px，bg `#98d4bb` / `#c7b8ea` / `#f4b8c5` / `#a8d8ea` / `#ffe6a7`（5色轮替），竖排文字 `writing-mode: vertical-rl`，圆角 0
- 行内标签：bg `#ffe6a7`（便签黄），文字 `#1a1a1a`，圆角 0，`font-style: italic`
- 装订孔：`width: 8px; height: 8px; border-radius: 50%; border: 1px solid #9a9590` 等距排列在左侧

*阴影层级：*
- L1 纸张：`2px 2px 0 #d4d0c8` — 纸张叠加，硬边
- L2 折页：`4px 4px 0 #d4d0c8` — 更厚的层叠
- L3 浮页：`6px 6px 12px rgba(0,0,0,0.08)` — 多页堆叠的自然阴影

*装饰线：*
- 分割线：`1px solid #d4d0c8`，左侧带装订孔对齐标记
- 页面边缘：`border-right: 1px dashed #d4d0c8`（模拟笔记本右边距线）

**设计规则：**

*推荐：*
- 纸张色(#f8f6f1)作为内容底色，深灰(#2d2d2d)作为外围
- 彩色标签(5色轮替)与章节一一对应，建立视觉导航
- 装订孔和虚线是"手写笔记"感觉的关键，不可省略

*避免：*
- 不要使用渐变或发光 — 本主题追求纸张质感
- 不要让标签颜色混用 — 每个章节固定一种颜色
- 不要使用 sans-serif 圆角按钮 — 保持编辑/文学气质

---

### 7. Pastel Geometry（柔粉几何）

**气质：** 友好、亲切、现代
**适用场景：** 团队汇报、内部分享、轻松场合

**字体：**
- 标题：`Plus Jakarta Sans` (700/800)
- 正文：`Plus Jakarta Sans` (400/500)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-primary: #c8d9e6;
    --card-bg: #faf9f7;
    --pill-pink: #f0b4d4;
    --pill-mint: #a8d4c4;
    --pill-sage: #5a7c6a;
    --pill-lavender: #9b8dc4;
}
```

**签名元素：**
- 圆角卡片 + 柔和阴影
- 右侧竖排彩色药丸标签
- 一致的药丸宽度，高低变化
- 柔和色调整体感

**组件样式：**

*按钮：*
- 主按钮：bg `#faf9f7`（白卡片色），文字 `#5a7c6a`（sage），圆角 999px（pill形），`box-shadow: 0 2px 8px rgba(0,0,0,0.08)`，hover 时 `transform: translateY(-2px)` + `box-shadow` 加深
- 彩色按钮：bg `#f0b4d4`（粉色），文字 `#ffffff`，圆角 999px，hover 时 bg `#a8d4c4`（薄荷）
- 幽灵按钮：bg transparent，文字 `#5a7c6a`，`1px solid rgba(90,124,106,0.3)`，圆角 999px

*卡片：*
- 普通卡片：bg `#faf9f7`，圆角 16px，`box-shadow: 0 4px 16px rgba(0,0,0,0.06)`，`border: none`
- 高亮卡片：bg `#faf9f7`，`border-left: 4px solid #a8d4c4`，圆角 16px，`box-shadow: 0 6px 20px rgba(168,212,196,0.15)`
- 数据卡片：bg `#faf9f7`，顶部 `border-top: 4px solid #f0b4d4`，数据数字用 `#5a7c6a` + Plus Jakarta Sans 800

*标签/徽章：*
- 药丸标签：圆角 999px，bg `#f0b4d4` / `#a8d4c4` / `#9b8dc4`（粉/薄荷/薰衣草），文字 `#ffffff`，`padding: 4px 12px`
- 竖排药丸：`writing-mode: vertical-rl`，宽 28px，高低错落，右侧定位
- 中性标签：bg `#c8d9e6`（背景色加深），文字 `#5a7c6a`，圆角 999px

*阴影层级：*
- L1 卡片：`0 2px 12px rgba(0,0,0,0.05)` — 柔和浮起，几乎无感
- L2 悬停：`0 6px 20px rgba(0,0,0,0.08)` — 轻微抬高
- L3 彩色辉光：`0 8px 24px rgba(168,212,196,0.2)` — 薄荷色调辉光，高亮焦点

*装饰线：*
- 分割线：`1px solid rgba(90,124,106,0.15)`，圆角端点
- 装饰点：`width: 6px; height: 6px; border-radius: 50%; background: #f0b4d4` 柔和色点排列

**设计规则：**

*推荐：*
- 药丸形(999px 圆角)是核心形状，按钮和标签统一使用
- 右侧竖排药丸标签高低错落，创造有机感
- 背景色(#c8d9e6)与卡片色(#faf9f7)的对比是基础

*避免：*
- 不要使用锐角或直角 — 本主题的气质是友好柔和
- 不要使用深色文字(#000) — 用 sage(#5a7c6a)保持柔和
- 不要让粉/薄荷/薰衣草三色竞争 — 每页选定一个主色

---

### 8. Paper & Ink（纸与墨）

**气质：** 编辑风格、文学感、深思熟虑
**适用场景：** 年度总结、深度报告、品牌故事

**字体：**
- 标题：`Cormorant Garamond` + `Source Serif 4`
- 正文：`Source Serif 4` (400)
- 中文回退：`'Noto Serif SC', 'Source Han Serif', serif`

**配色：**
```css
:root {
    --bg-cream: #faf9f7;
    --text-primary: #1a1a1a;
    --text-secondary: #555555;
    --accent-crimson: #c41e3a;
}
```

**签名元素：**
- 首字下沉（Drop Caps）
- 引用块（Pull Quotes）
- 优雅水平分割线
- 深红强调色

**组件样式：**

*按钮：*
- 主按钮：bg `#1a1a1a`（墨色），文字 `#faf9f7`（纸色），圆角 4px，hover 时 `border-bottom: 2px solid #c41e3a`（深红下划线，像笔划标注）
- 强调按钮：bg `#c41e3a`（深红），文字 `#faf9f7`，圆角 4px，hover 时 `filter: brightness(1.1)`
- 幽灵按钮：bg transparent，文字 `#1a1a1a`，`1px solid #555555`，圆角 4px，hover 时文字变 `#c41e3a`

*卡片：*
- 普通卡片：bg `#faf9f7`，圆角 0，`box-shadow: inset 0 0 0 1px #ddd`（内凹阴影，像纸张压痕），`border: none`
- 引用卡片：bg `#faf9f7`，左侧 `border-left: 3px solid #c41e3a`，首字母 `float: left; font-size: 3em; line-height: 0.8; color: #c41e3a`（首字下沉效果）
- 数据卡片：bg `#ffffff`，`border: 1px solid #e8e6dc`，数据数字用 Cormorant Garamond 600 + `#c41e3a`

*标签/徽章：*
- 圆角 0（方正，文学感），bg `rgba(196,30,58,0.08)`，文字 `#c41e3a`，字体 Source Serif 4 italic
- 注释标签：bg `#f0eee6`（旧纸色），文字 `#555555`，左侧有 `em-dash` 装饰 `—`
- 章节标记：纯文字 `font-variant: small-caps`，`letter-spacing: 2px`，`color: #c41e3a`

*阴影层级：*
- L1 纸压：`inset 0 0 0 1px #ddd` — 内凹，像纸上的压痕
- L2 页浮：`0 2px 12px rgba(0,0,0,0.06)` — 轻微浮起
- L3 深红投影：`0 4px 16px rgba(196,30,58,0.08)` — 淡红色调阴影

*装饰线：*
- 分割线：`1px solid #c41e3a`，宽度 40px（短而精），居中，两端可加小菱形装饰 `◆`
- 引用分割：`3px solid #c41e3a`，左侧定位

**设计规则：**

*推荐：*
- 深红(#c41e3a)是唯一的视觉焦点，用于首字下沉和关键标注
- 内凹阴影(inset shadow)模拟纸张压痕，是本主题独有的深度语言
- 分割线短而精(40px)，居中，可加菱形装饰

*避免：*
- 不要使用 sans-serif 字体 — 必须保持衬线字体的文学气质
- 不要使用圆角 — 纸与墨是方正的，border-radius: 0
- 不要让深红大面积出现 — 它是笔划标注，不是背景色

---

## 特色主题

### 9. Business Blue（商务蓝）

**气质：** 专业、可信赖、稳重
**适用场景：** 商务汇报、企业介绍、政府报告

**字体：**
- 标题：`Outfit` (700/800)
- 正文：`Outfit` (400/500)
- 中文回退：`'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', sans-serif`

**配色：**
```css
:root {
    --primary: #1E3A8A;
    --secondary: #3B82F6;
    --accent: #F59E0B;
    --bg: #F8FAFC;
    --text: #1E293B;
}
```

**签名元素：**
- 深蓝主色 + 琥珀金强调
- 固定顶部导航栏
- 卡片式数据展示
- 数据放大显示

**组件样式：**

*按钮：*
- 主按钮：bg `#1E3A8A`（深蓝），文字 `#ffffff`，圆角 6px，hover 时 bg `#3B82F6`（亮蓝），`box-shadow: 0 4px 12px rgba(30,58,138,0.3)`
- 强调按钮：bg `#F59E0B`（琥珀金），文字 `#1E293B`，圆角 6px，hover 时 `filter: brightness(1.1)` + `box-shadow: 0 4px 12px rgba(245,158,11,0.3)`
- 幽灵按钮：bg transparent，文字 `#1E3A8A`，`1px solid #1E3A8A`，圆角 6px，hover 时 bg `rgba(30,58,138,0.05)`

*卡片：*
- 普通卡片：bg `#ffffff`，圆角 8px，`border: 1px solid #e2e8f0`，`box-shadow: 0 2px 8px rgba(30,58,138,0.06)`
- 高亮卡片：bg `#ffffff`，`border-left: 4px solid #1E3A8A`，圆角 8px，`box-shadow: 0 4px 16px rgba(30,58,138,0.1)`
- 数据卡片：bg `#ffffff`，`border-top: 3px solid #3B82F6`，数据数字用 `#1E3A8A` + Outfit 800，底部金色下划线装饰

*标签/徽章：*
- 圆角 4px（稳重方正），bg `rgba(30,58,138,0.08)`，文字 `#1E3A8A`，字体 Outfit 500
- 金色标签：bg `rgba(245,158,11,0.1)`，文字 `#F59E0B`，圆角 4px
- 状态标签：小圆点 + 文字，`::before { content: ''; width:6px; height:6px; border-radius:50%; background:#10B981 }`

*阴影层级：*
- L1 卡片：`0 2px 8px rgba(30,58,138,0.06)` — 浅蓝阴影
- L2 悬停：`0 4px 16px rgba(30,58,138,0.1)` — 蓝色调浮起
- L3 金色焦点：`0 8px 24px rgba(245,158,11,0.12)` — 琥珀金辉光，用于CTA

*装饰线：*
- 分割线：`1px solid #e2e8f0`，可左侧加蓝色圆点标记
- 导航下划线：`2px solid #3B82F6`，用于活跃导航项

**设计规则：**

*推荐：*
- 深蓝(#1E3A8A)与琥珀金(#F59E0B)严格分工：蓝=结构/信任，金=行动/强调
- 数据卡片使用 Outfit 800 放大数字，搭配金色下划线装饰
- 状态标签用小圆点+文字组合，传达专业感

*避免：*
- 不要使用渐变 — 商务蓝追求稳重，不是花哨
- 不要让琥珀金和深蓝出现在同一按钮上
- 不要使用过大的圆角 — 保持 4-8px 的稳重方正感

---

### 10. Data Dark（数据暗色）

**气质：** 数据突出、高对比、科技感
**适用场景：** 数据分析报告、技术复盘、量化展示

**字体：**
- 标题：`JetBrains Mono` (700)
- 正文：`JetBrains Mono` (400)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --primary: #0F172A;
    --secondary: #10B981;
    --accent: #F43F5E;
    --bg: #1E293B;
    --text: #E2E8F0;
}
```

**签名元素：**
- 终端风格等宽字体
- 翡翠绿 + 玫瑰红数据高亮
- 深色背景最大化数据对比度
- 代码块风格的数据展示

**组件样式：**

*按钮：*
- 主按钮：bg `#10B981`，文字 `#0F172A`，圆角 2px（直角终端感），`font-family: JetBrains Mono`，hover 时 `box-shadow: 0 0 12px rgba(16,185,129,0.4)` 绿色发光
- 强调/危险按钮：bg `#F43F5E`，文字 `#E2E8F0`，圆角 2px，hover 时 `box-shadow: 0 0 12px rgba(244,63,94,0.4)` 玫瑰红发光
- 幽灵按钮：bg transparent，文字 `#10B981`，`1px solid rgba(16,185,129,0.3)`，hover 时 bg `rgba(16,185,129,0.1)`

*卡片：*
- 普通卡片：bg `#0F172A`，圆角 4px，`border: 1px solid rgba(16,185,129,0.15)`，顶栏 3px 色带 `border-top: 3px solid #10B981`（终端窗口风格）
- 高亮卡片：bg `#0F172A`，`border-top: 3px solid #F43F5E`，`box-shadow: 0 0 20px rgba(16,185,129,0.08)` 数据聚焦发光
- 数据卡片：bg `#0F172A`，顶栏色带 + 内部行号 `rgba(16,185,129,0.06)` 交替行背景，数据数字用 `#10B981` 或 `#F43F5E` 大字号，`font-family: JetBrains Mono`

*标签/徽章：*
- 正向标签（增长/成功）：bg `rgba(16,185,129,0.15)`，文字 `#10B981`，圆角 2px，`font-family: JetBrains Mono`，可加 `+` 前缀
- 负向标签（下降/警告）：bg `rgba(244,63,94,0.15)`，文字 `#F43F5E`，圆角 2px，可加 `-` 前缀
- 中性标签：bg `rgba(226,232,240,0.1)`，文字 `#8B949E`，圆角 2px

*阴影层级：*
- L1 基础：`0 2px 8px rgba(0,0,0,0.4)` — 深色卡片浮起
- L2 发光：`0 0 16px rgba(16,185,129,0.12)` — 翡翠绿微辉光，用于 hover 卡片
- L3 强发光：`0 0 24px rgba(16,185,129,0.2), 0 0 48px rgba(244,63,94,0.08)` — 双色辉光，封面焦点数据

*装饰线：*
- 分割线：`1px solid rgba(16,185,129,0.2)`，左侧可加 `# ` 前缀模拟注释行
- 代码块行号线：`border-left: 2px solid rgba(16,185,129,0.1)`

**设计规则：**

*推荐：*
- 翡翠绿(#10B981)=正向/成功，玫瑰红(#F43F5E)=负向/警告，严格语义分工
- 终端窗口风格卡片(顶栏色带+行号背景)是核心签名
- 等宽字体(JetBrains Mono)用于所有数据和标签

*避免：*
- 不要使用 sans-serif 字体显示数据 — 必须等宽
- 不要引入第三种高亮色 — 绿/红双色系统是核心约束
- 不要使用大圆角 — 保持 2-4px 的终端直角感

---

### 11. Gradient Fashion（渐变时尚）

**气质：** 现代、活力、潮流
**适用场景：** 品牌发布、创意提案、时尚行业

**字体：**
- 标题：`Syne` (700/800)
- 正文：`Space Mono` (400)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    --text-primary: #ffffff;
    --accent: #FBBF24;
    --card-bg: rgba(255,255,255,0.15);
}
```

**签名元素：**
- 紫蓝渐变背景
- 毛玻璃卡片效果
- 亮黄强调色
- 闪电/能量线条装饰

**组件样式：**

*按钮：*
- 主按钮：bg `linear-gradient(135deg, #667eea, #764ba2)`（主题渐变），文字 `#ffffff`，圆角 999px（pill形），hover 时 `box-shadow: 0 0 20px rgba(102,126,234,0.4)` + `transform: scale(1.03)`
- 强调按钮：bg `#FBBF24`（亮黄），文字 `#1a1a1a`，圆角 999px，hover 时 `box-shadow: 0 0 24px rgba(251,191,36,0.5)` 黄色发光
- 幽灵按钮：bg `rgba(255,255,255,0.1)`，文字 `#ffffff`，`1px solid rgba(255,255,255,0.25)`，圆角 999px，hover 时 bg `rgba(255,255,255,0.2)`

*卡片：*
- 普通卡片：bg `rgba(255,255,255,0.15)`，圆角 16px，`backdrop-filter: blur(12px)`，`border: 1px solid rgba(255,255,255,0.2)`（毛玻璃）
- 高亮卡片：bg `rgba(255,255,255,0.2)`，`border: 1px solid rgba(251,191,36,0.3)`，`box-shadow: 0 0 24px rgba(251,191,36,0.15)` 黄色辉光
- 数据卡片：bg `rgba(255,255,255,0.12)`，数据数字用 `#FBBF24` 超大字号 + Syne 800，渐变文字 `background: linear-gradient(135deg, #667eea, #FBBF24)`

*标签/徽章：*
- pill 形（999px），bg `rgba(255,255,255,0.15)`，文字 `#ffffff`，`border: 1px solid rgba(255,255,255,0.2)`
- 黄色标签：bg `rgba(251,191,36,0.2)`，文字 `#FBBF24`，pill 形
- 能量标签：左侧 `border-left: 3px solid #FBBF24`，带闪电装饰

*阴影层级：*
- L1 毛玻璃：`0 4px 16px rgba(0,0,0,0.2)` — 半透明底上的深度
- L2 紫蓝辉光：`0 0 20px rgba(102,126,234,0.3)` — 渐变色光晕
- L3 双色辉光：`0 0 30px rgba(102,126,234,0.25), 0 0 60px rgba(118,75,162,0.15)` — 紫蓝双层发光

*装饰线：*
- 分割线：`1px solid rgba(255,255,255,0.15)`，可替换为闪电锯齿 SVG
- 能量条：`height: 2px; background: linear-gradient(90deg, #667eea, #FBBF24, #764ba2)` 渐变能量线

**设计规则：**

*推荐：*
- 紫蓝渐变(#667eea→#764ba2)用于背景和主按钮，是主题DNA
- 毛玻璃效果(backdrop-filter: blur)应用于所有卡片
- 亮黄(#FBBF24)是提亮色，用于数据和强调标签

*避免：*
- 不要使用实色背景 — 本主题依赖渐变和半透明
- 不要让亮黄和渐变背景同时占据大面积 — 黄色是点缀
- 不要使用直角或小圆角 — 保持 16px+ 的圆润感

---

### 12. Terminal Green（终端绿）

**气质：** 极客、黑客美学、开发者专属
**适用场景：** 技术分享、开发者大会、代码演示

**字体：**
- 全局：`JetBrains Mono` (400/700) — 纯等宽
- 中文回退：`'Noto Sans SC', 'PingFang SC', monospace`

**配色：**
```css
:root {
    --bg-primary: #0d1117;
    --accent-green: #39d353;
    --text-primary: #c9d1d9;
    --text-secondary: #8b949e;
}
```

**签名元素：**
- 扫描线效果
- 闪烁光标
- 代码语法高亮风格
- 命令行提示符装饰

**组件样式：**

*按钮：*
- 主按钮：bg `rgba(57,211,83,0.12)`，文字 `#39d353`，圆角 2px（直角终端感），`border: 1px solid rgba(57,211,83,0.3)`，前缀 `▶` 装饰，hover 时 `box-shadow: 0 0 12px rgba(57,211,83,0.3)`
- 执行按钮：bg `#39d353`，文字 `#0d1117`，圆角 2px，`font-family: JetBrains Mono`，hover 时 `filter: brightness(1.1)` + 绿色发光
- 幽灵按钮：bg transparent，文字 `#8b949e`，`1px solid #30363d`，圆角 2px，hover 时文字变 `#39d353`

*卡片：*
- 普通卡片：bg `#161b22`，圆角 4px，`border: 1px solid #30363d`，顶栏 `padding: 8px 12px; border-bottom: 1px solid #30363d; background: #0d1117`（终端窗口头，可放3个圆点 `● ● ●`）
- 高亮卡片：bg `#161b22`，`border: 1px solid rgba(57,211,83,0.3)`，`box-shadow: 0 0 16px rgba(57,211,83,0.08)` 绿色微辉光，行号 `border-right: 1px solid #30363d; padding-right: 12px; color: #484f58`
- 数据卡片：bg `#0d1117`，`border: 1px solid #30363d`，数据数字用 `#39d353` + JetBrains Mono 700，前缀 `$` 命令行提示符

*标签/徽章：*
- 直角（border-radius: 0 或 2px），bg `rgba(57,211,83,0.12)`，文字 `#39d353`，`font-family: JetBrains Mono`
- 注释标签：文字 `#8b949e`，前缀 `// `（注释风格）
- 语法标签：模拟语法高亮 — 关键字 `#ff7b72`（红），字符串 `#a5d6ff`（蓝），数字 `#79c0ff`
- commit 标签：bg `rgba(57,211,83,0.15)`，文字 `#39d353`，前缀 `●`（类似 git status）

*阴影层级：*
- L1 终端：`0 2px 8px rgba(0,0,0,0.5)` — 深色内凹感
- L2 绿辉：`0 0 16px rgba(57,211,83,0.1)` — 绿色微光
- L3 扫描辉光：`0 0 24px rgba(57,211,83,0.15), 0 0 48px rgba(57,211,83,0.05)` — CRT 显示器辉光效果

*装饰线：*
- 分割线：`1px solid #30363d`，可加注释 `// ────────────`
- 扫描线：`background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px)` 覆盖全屏
- 光标：`border-right: 2px solid #39d353; animation: blink 1s step-end infinite` 闪烁光标效果

**设计规则：**

*推荐：*
- 绿色(#39d353)用于成功/执行/核心数据，灰色(#8b949e)用于辅助信息
- 终端窗口头(三圆点+暗栏)是每张卡片的标配
- 命令行提示符($)和注释前缀(//)是签名装饰元素

*避免：*
- 不要引入彩色高亮 — 本主题严格限制在绿/灰/暗色范围
- 不要使用 sans-serif 或 serif 字体 — 必须等宽
- 不要让背景亮于 #161b22 — 深色是终端的基础

---

### 13. Ink Garden（水墨雅集）

**气质：** 古典雅致、温润书卷、诗意留白
**适用场景：** 文化报告、政府汇报、教育演讲、传统产业展示、茶道/书法/国画主题

**字体：**
- 标题：`Noto Serif SC` (600/700) — 宋体衬线，古典庄重
- 正文：`Noto Sans SC` (400/500) — 清晰无衬线，阅读舒适
- 中文回退：`'PingFang SC', 'Microsoft YaHei', 'SimSun', serif`
- 英文回退：`'Cormorant Garamond', serif`

**配色：**
```css
:root {
    --bg-primary: #f5f0e8;            /* 宣纸米白 */
    --bg-gradient: linear-gradient(135deg, #f5f0e8 0%, #ede7d9 50%, #f0eadb 100%);
    --bg-dark: #2c2420;               /* 深褐墨色（深色页） */
    --bg-card: #faf7f2;               /* 浅宣纸（卡片底） */
    --text-heading: #2a1f18;          /* 浓墨标题 */
    --text-body: #5c4d3c;             /* 淡墨正文 */
    --accent: #c8963e;                /* 赤金（主强调） */
    --accent-light: rgba(200,150,62,0.10);
    --accent-secondary: #a06426;      /* 古铜（辅强调） */
    --border: #d8cdb8;                /* 宣纸灰边 */
    --ink-wash: rgba(42,31,24,0.06);  /* 水墨晕染色 */
    --seal-red: #c23a2b;              /* 印章朱砂红 */
}
```

**签名元素：**
- 水墨晕染背景纹理（CSS radial-gradient 模拟墨迹扩散）
- 银杏叶 SVG 装饰（金色描边，`opacity: 0.15`）
- 印章风格圆形编号（朱砂红圆 + 白色篆字壹贰叁）
- 竖排标题变体（`writing-mode: vertical-rl`，仅封面可选）
- 古典花边框（双线 border + 角花纹 SVG）

**组件样式：**

*按钮：*
- 主按钮：bg `#c8963e`，白字 `#ffffff`，圆角 4px，`font-weight: 600`（Noto Serif SC），hover 时 `filter: brightness(1.1)` + `transform: translateY(-2px)`
- 次按钮：bg `transparent`，`#c8963e` 文字，`1px solid #c8963e`，圆角 4px，hover 时 `bg rgba(200,150,62,0.08)`
- 印章按钮：bg `#c23a2b`（朱砂红），白字，圆角 50%（圆形），`min-width: 44px`，`min-height: 44px`，`font-family: serif`，hover 时 `box-shadow: 0 0 12px rgba(194,58,43,0.4)`

*卡片：*
- 普通卡片：bg `#faf7f2`，圆角 4px，`border: 1px solid #d8cdb8`，`box-shadow: 0 2px 8px rgba(42,31,24,0.06)`
- 高亮/焦点卡片：bg `linear-gradient(135deg, rgba(200,150,62,0.08), rgba(200,150,62,0.03))`，`border: 1px solid rgba(200,150,62,0.25)`，左侧 `3px solid #c8963e`，标题用 Noto Serif SC 700
- 数据卡片：bg `#faf7f2`，`border-top: 2px solid #c8963e`，数据数字用 `#2a1f18` + `font-size: 2.5rem` + Noto Serif SC 600 + `letter-spacing: -0.5px`

*标签/徽章：*
- 方角标签（`border-radius: 2px`），bg `rgba(200,150,62,0.10)`，`#a06426` 文字，`font-weight: 500`（Noto Sans SC），`letter-spacing: 1px`
- 印章编号标签：bg `#c23a2b` 圆形（32px），白字篆体，`font-family: serif`，对应壹/贰/叁/肆/伍
- 暗色标签：bg `rgba(42,31,24,0.06)`，`#5c4d3c` 文字，`border: 1px solid #d8cdb8`

*阴影层级：*
- L1 浮墨：`0 2px 8px rgba(42,31,24,0.06)` — 宣纸上的微妙浮起
- L2 重墨：`0 4px 20px rgba(42,31,24,0.10)` — 普通卡片和交互元素
- L3 金晕：`0 8px 32px rgba(200,150,62,0.15), 0 2px 8px rgba(42,31,24,0.08)` — 赤金焦点卡片，带暖色光晕

*装饰线：*
- 分割线：`2px solid #c8963e`（赤金线），长度不超过容器 30%，左对齐
- 双线边框：外层 `1px solid #d8cdb8` + 内层 `1px solid #d8cdb8`（间距 4px），用于内容框
- 水墨晕染：`radial-gradient(ellipse at 80% 20%, rgba(42,31,24,0.04), transparent 60%)` — 角落装饰

**设计规则：**

*推荐：*
- 赤金(#c8963e)作为唯一强调色，每页最多2处金色高亮
- 大面积留白 — "计白当黑"，内容区不超过页面60%
- 使用水墨晕染效果（低不透明度 radial-gradient）营造纸面质感
- 封面可用竖排标题（`writing-mode: vertical-rl`）增加古典仪式感
- 数据页用印章编号替代阿拉伯数字，增强风格一致性

*避免：*
- 不要使用饱和度高的颜色 — 所有颜色必须带暖灰/棕底
- 不要使用纯白(#fff)背景 — 用宣纸米白(#f5f0e8)代替
- 不要使用现代几何装饰（圆点、三角、渐变色块）— 只用水墨、银杏、印章等传统元素
- 不要使用圆角超过 8px — 古典风格偏向方正（4px 为主）
- 不要同时使用赤金和朱砂红作为强调 — 两者互斥，赤金为主、朱砂仅限印章

---

## 品牌风格

### 14. Apple Minimalist（Apple 极简）

**气质：** 极简、高端、产品即焦点
**适用场景：** 产品发布、高端展示、品牌提案

**字体：**
- 标题：`Sora` (600) — 几何风格，接近 SF Pro
- 正文：`Sora` (400)
- 中文回退：`'Noto Sans SC', 'PingFang SC', 'Microsoft YaHei', sans-serif`

**配色：**
```css
:root {
    --bg-dark: #000000;
    --bg-light: #f5f5f7;
    --text-dark: #1d1d1f;
    --text-light: #ffffff;
    --accent-blue: #0071e3;
}
```

**签名元素：**
- 黑白分块交替的"电影节奏"
- 产品级大图占据全屏背景
- 980px 圆角药丸形 CTA 链接
- 极紧标题行高(1.07-1.14)

**组件样式：**

*按钮：*
- 主按钮：bg `#0071e3`，白字，圆角 980px（药丸形），hover 时 `opacity: 0.88`
- 次按钮：bg `#1d1d1f`，白字，圆角 980px，hover 时 `opacity: 0.88`
- 药丸链接：bg transparent，文字 `#0071e3`（亮底）或 `#2997ff`（暗底），`→` 箭头装饰，hover 时 `text-decoration: underline`

*卡片：*
- 普通卡片：bg `#f5f5f7`（亮）或 `#272729`（暗），圆角 12px，`border: none`，`box-shadow: rgba(0,0,0,0.22) 3px 5px 30px`
- 高亮卡片：bg `#000000`，文字 `#ffffff`，圆角 12px，产品图占据 60-70%
- 数据卡片：bg `#f5f5f7`，数据数字用 `#1d1d1f` + Sora 600 + `font-size: 3rem`

*标签/徽章：*
- 圆角 8px，bg `#0071e3`，白字，`font-weight: 400`
- 药丸标签：圆角 980px，bg transparent，文字 `#0071e3`，`1px solid #0066cc`
- 中性标签：bg `#f5f5f7`，文字 `rgba(0,0,0,0.8)`，圆角 8px

*阴影层级：*
- L1 卡片：无阴影 — Apple 风格极少使用阴影
- L2 悬浮：`rgba(0,0,0,0.22) 3px 5px 30px` — 仅用于产品卡片
- L3 导航毛玻璃：`backdrop-filter: saturate(180%) blur(20px)` 在 `rgba(0,0,0,0.8)` 上

*装饰线：*
- 分割线：极少使用，依靠背景色切换(黑↔浅灰)分隔区域
- 导航下划线：hover 时出现 `1px solid #ffffff`

**设计规则：**

*推荐：*
- 黑色(#000000)和浅灰(#f5f5f7)交替背景创造"电影节奏" — 这是 Apple 最有辨识度的特征
- #0071e3 蓝色仅用于可交互元素(链接、按钮)，永远不做装饰色
- 标题使用极紧行高(1.07)和负字间距(-0.28px at 56px)

*避免：*
- 不要引入第二种彩色强调色 — 全局只有蓝+黑白
- 不要使用粗体(700以上) — Apple 最高用到 600(semibold)
- 不要给卡片加边框 — 苹果几乎不使用可见边框

---

### 15. Stripe Professional（Stripe 专业）

**气质：** 精密、自信、轻量级权威
**适用场景：** 金融科技、B2B 产品、SaaS 汇报

**字体：**
- 标题：`Work Sans` (300) — 轻量标题是 Stripe 的灵魂
- 正文：`Work Sans` (400)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-primary: #ffffff;
    --bg-dark: #1c1e54;
    --text-heading: #061b31;
    --text-body: #64748d;
    --accent-purple: #533afd;
}
```

**签名元素：**
- weight 300 轻量级标题 — "不靠粗体发声"
- 蓝色调多层阴影 rgba(50,50,93,0.25)
- 深靛蓝(#1c1e54)暗色区块
- 4-8px 保守圆角

**组件样式：**

*按钮：*
- 主按钮：bg `#533afd`，白字，圆角 4px，hover 时 bg `#4434d4`
- 幽灵按钮：bg transparent，文字 `#533afd`，`1px solid #b9b9f9`，圆角 4px，hover 时 bg `rgba(83,58,253,0.05)`
- 信息按钮：bg transparent，文字 `#061b31`，`1px solid #e5edf5`，圆角 4px

*卡片：*
- 普通卡片：bg `#ffffff`，圆角 6px，`border: 1px solid #e5edf5`，`box-shadow: rgba(50,50,93,0.25) 0px 30px 45px -30px, rgba(0,0,0,0.1) 0px 18px 36px -18px`
- 暗色卡片：bg `#1c1e54`，文字 `#ffffff`，圆角 6px，`border: 1px solid rgba(255,255,255,0.1)`
- 数据卡片：bg `#ffffff`，数据数字用 `#061b31` + Work Sans 300 + `font-size: 3rem`

*标签/徽章：*
- 圆角 4px，bg `rgba(83,58,253,0.08)`，文字 `#533afd`，Work Sans 400
- 成功标签：bg `rgba(21,190,83,0.2)`，文字 `#108c3d`，圆角 4px，`1px solid rgba(21,190,83,0.4)`
- 中性标签：bg `#ffffff`，文字 `#64748d`，圆角 4px，`border: 1px solid #e5edf5`

*阴影层级：*
- L1 氛围：`rgba(23,23,23,0.06) 0px 3px 6px` — 微弱浮起
- L2 标准：`rgba(23,23,23,0.08) 0px 15px 35px` — 标准卡片
- L3 蓝调：`rgba(50,50,93,0.25) 0px 30px 45px -30px, rgba(0,0,0,0.1) 0px 18px 36px -18px` — 蓝色调深度

*装饰线：*
- 分割线：`1px solid #e5edf5`
- 虚线装饰：`1px dashed #362baa` 用于占位区域
- 暗色区块边框：`1px solid rgba(255,255,255,0.1)`

**设计规则：**

*推荐：*
- 标题使用 weight 300 — 轻量级是 Stripe 的核心辨识度
- 所有阴影带蓝色调 rgba(50,50,93,0.25) — 阴影也是品牌的一部分
- 标题颜色使用 #061b31(深靛蓝)而非纯黑 — 这点微妙但重要

*避免：*
- 不要使用 weight 600-700 的粗体标题 — 300 是品牌声音
- 不要使用大于 8px 的圆角 — Stripe 追求保守精准
- 不要让品红/宝石红出现在按钮上 — 它们只用于装饰渐变

---

### 16. Linear Dark（Linear 深色）

**气质：** 暗色优先、精密工程、开发者工具美学
**适用场景：** 开发者工具、技术产品、项目管理

**字体：**
- 标题：`Inter` (510 — 介于常规和中等之间) — `font-variation-settings: "wght" 510`
- 正文：`Inter` (400)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-marketing: #08090a;
    --bg-panel: #0f1011;
    --bg-surface: #191a1b;
    --text-primary: #f7f8f8;
    --text-secondary: #d0d6e0;
    --text-muted: #8a8f98;
    --accent-indigo: #5e6ad2;
}
```

**签名元素：**
- 暗色原生画布 — 从 #08090a 到 #191a1b 的亮度阶梯
- weight 510 — "介于常规和中等之间"的独有字重
- 半透明白色边框 rgba(255,255,255,0.05-0.08)
- Indigo(#5e6ad2)是唯一的彩色

**组件样式：**

*按钮：*
- 主按钮：bg `#5e6ad2`，白字，圆角 6px，hover 时 bg `#828fff`
- 幽灵按钮：bg `rgba(255,255,255,0.02)`，文字 `#e2e4e7`，`1px solid rgb(36,40,44)`，圆角 6px
- 药丸按钮：bg transparent，文字 `#d0d6e0`，圆角 9999px，`1px solid rgb(35,37,42)`

*卡片：*
- 普通卡片：bg `rgba(255,255,255,0.02)`，圆角 8px，`border: 1px solid rgba(255,255,255,0.08)`
- 高亮卡片：bg `rgba(255,255,255,0.05)`，`border: 1px solid rgba(255,255,255,0.08)`，圆角 12px
- 数据卡片：bg `#191a1b`，数据数字用 `#f7f8f8` + Inter 510 + `font-size: 3rem`

*标签/徽章：*
- 药丸标签：bg transparent，文字 `#d0d6e0`，圆角 9999px，`1px solid rgb(35,37,42)`，Inter 510
- 成功标签：bg `#10b981`，文字 `#f7f8f8`，圆角 50%
- 微标签：bg `rgba(255,255,255,0.05)`，文字 `#f7f8f8`，圆角 2px，`border: 1px solid rgba(255,255,255,0.05)`

*阴影层级：*
- L1 微浮：`rgba(0,0,0,0.03) 0px 1.2px 0px`
- L2 表面：bg 透明度递增(0.02→0.04→0.05) + 半透明边框
- L3 悬浮：`rgba(0,0,0,0.4) 0px 2px 4px`

*装饰线：*
- 分割线：`1px solid rgba(255,255,255,0.05)` — 呢喃级分割
- 边框：`1px solid rgba(255,255,255,0.08)` — 标准级

**设计规则：**

*推荐：*
- weight 510 是核心辨识度 — 它创造了"轻微强调但不喊叫"的独特感觉
- 暗色表面通过 rgba(255,255,255, 0.02→0.04→0.05) 递增来表示层级
- 主文字使用 #f7f8f8 而非纯白 #ffffff — 防止暗底上的视觉刺眼

*避免：*
- 不要使用纯白(#ffffff)作为主文字色 — #f7f8f8 更舒适
- 不要使用不透明背景色做按钮 — 透明度系统是核心
- 不要使用超过 590 的字重 — Linear 的极限是 590

---

### 17. Tesla Pure（Tesla 纯净）

**气质：** 极致减法、产品即设计、画廊美学
**适用场景：** 产品展示、硬件发布、汽车/工业

**字体：**
- 标题：`Figtree` (500) — 干净几何感
- 正文：`Figtree` (400)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-primary: #FFFFFF;
    --bg-alt: #F4F4F4;
    --text-primary: #171A20;
    --text-secondary: #393C41;
    --text-muted: #5C5E62;
    --accent-blue: #3E6AE1;
}
```

**签名元素：**
- 零装饰 — 无阴影、无渐变、无图案
- 全屏摄影图背景(100vh)
- 单一蓝(#3E6AE1)仅用于主 CTA
- 所有交互过渡统一 0.33s cubic-bezier

**组件样式：**

*按钮：*
- 主按钮：bg `#3E6AE1`，白字，圆角 4px，`min-height: 40px`，`transition: all 0.33s`
- 次按钮：bg `#FFFFFF`，文字 `#393C41`，圆角 4px，hover 时微暗
- 文字链接：文字 `#5C5E62`，hover 时 `text-decoration: underline`，`transition: 0.33s`

*卡片：*
- 普通卡片：bg transparent，`border: none`，`box-shadow: none` — 完全零装饰
- 分类卡片：全幅摄影图背景，圆角 12px，`overflow: hidden`，白色文字叠加
- 数据卡片：bg `#FFFFFF`，数据数字用 `#171A20` + Figtree 500

*标签/徽章：*
- 极简标签：bg transparent，文字 `#5C5E62`，圆角 4px
- 蓝色标签：bg `#3E6AE1`，白字，圆角 4px
- 无标签装饰 — 信息通过间距和字重传达

*阴影层级：*
- L0：无阴影 — 全局零阴影策略
- L1 霜玻璃：`rgba(255,255,255,0.75)` + `backdrop-filter` 仅用于导航
- L2 微弱：`rgba(0,0,0,0.05)` — 极罕见

*装饰线：*
- 分割线：极少使用，区域通过留白分隔
- 边框：`1px solid #EEEEEE` 仅用于极淡分隔

**设计规则：**

*推荐：*
- 让图片/内容承担所有视觉重量 — UI 本身应该是"隐形"的
- 每屏只传达一个信息 — 全屏区域是画廊式体验
- 所有动画过渡统一 0.33s — 一致性延伸到运动层面

*避免：*
- 不要添加任何阴影 — Tesla 的美学是"零阴影"
- 不要使用第二种彩色 — 只有蓝色一种强调色
- 不要使用超过 500 的字重 — Tesla 最低限度用字重表达

---

### 18. Vercel Engineering（Vercel 工程）

**气质：** 极致精密、开发者基础设施美学
**适用场景：** 开发者工具、基础设施、技术品牌

**字体：**
- 标题：`Geist Sans` (600) — 极端负字间距(-2.4px at 48px)
- 正文：`Geist Sans` (400)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-primary: #ffffff;
    --text-primary: #171717;
    --text-secondary: #4d4d4d;
    --accent-black: #171717;
    --border-shadow: rgba(0,0,0,0.08) 0px 0px 0px 1px;
}
```

**签名元素：**
- 阴影即边框：`box-shadow: 0px 0px 0px 1px rgba(0,0,0,0.08)` 替代 CSS border
- 极端负字间距(-2.4px at 48px)
- 工作流三色：开发蓝(#0a72ef) / 预览粉(#de1d8d) / 发布红(#ff5b4f)
- 内部 #fafafa 光环

**组件样式：**

*按钮：*
- 主按钮：bg `#171717`，白字，圆角 6px，hover 时 `filter: brightness(1.2)`
- 白色按钮：bg `#ffffff`，文字 `#171717`，圆角 6px，`box-shadow: rgb(235,235,235) 0px 0px 0px 1px`
- 药丸标签按钮：bg `#ebf5ff`，文字 `#0068d6`，圆角 9999px

*卡片：*
- 普通卡片：bg `#ffffff`，圆角 8px，无 CSS border，`box-shadow: rgba(0,0,0,0.08) 0px 0px 0px 1px, rgba(0,0,0,0.04) 0px 2px 2px, #fafafa 0px 0px 0px 1px inset`
- 图片卡片：`1px solid #ebebeb` + 顶部圆角 `12px 12px 0px 0px`
- 数据卡片：bg `#ffffff`，数据数字用 `#171717` + Geist 600 + `font-size: 3rem` + `letter-spacing: -1.28px`

*标签/徽章：*
- 药丸标签：bg `#ebf5ff`，文字 `#0068d6`，圆角 9999px，Geist 500 12px
- 工作流标签：文字色对应阶段(#0a72ef / #de1d8d / #ff5b4f)，Geist Mono uppercase
- 中性标签：bg transparent，文字 `#4d4d4d`

*阴影层级：*
- L1 环形边框：`rgba(0,0,0,0.08) 0px 0px 0px 1px` — 替代传统边框
- L2 标准卡：环形 + `rgba(0,0,0,0.04) 0px 2px 2px`
- L3 完整卡：环形 + 柔和 + `rgba(0,0,0,0.04) 0px 8px 8px -8px` + 内部 `#fafafa` 环形

*装饰线：*
- 分割线：`1px solid #171717` — 深色实线分隔大区域
- 边框全部用 shadow 实现，不用 CSS border 属性
- 底部分隔：`border-bottom: 1px solid #171717`

**设计规则：**

*推荐：*
- 用阴影即边框(`0px 0px 0px 1px`)替代所有 CSS border — 这是 Vercel 最核心技术特征
- 标题使用极端负字间距(-2.4px at 48px) — 创造"压缩代码"的视觉隐喻
- 卡片阴影的内部 #fafafa 环形是让卡片"内发光"的关键

*避免：*
- 不要使用传统 CSS border — 一律用 shadow 技术替代
- 不要使用超过 600 的字重 — Vercel 的极限是 600
- 不要将工作流三色(蓝/粉/红)用于装饰 — 它们只标记管道阶段

---

### 19. Notion Warm（Notion 温暖）

**气质：** 温暖极简、纸质感、工具即画布
**适用场景：** 知识管理、协作工具、SaaS 展示

**字体：**
- 标题：`Inter` (700) — `font-feature-settings: "lnum"`
- 正文：`Inter` (400)
- 中文回退：`'Noto Sans SC', 'PingFang SC', sans-serif`

**配色：**
```css
:root {
    --bg-primary: #ffffff;
    --bg-warm: #f6f5f4;
    --text-primary: rgba(0,0,0,0.95);
    --text-secondary: #615d59;
    --text-muted: #a39e98;
    --accent-blue: #0075de;
}
```

**签名元素：**
- 温暖中性色 — 所有灰色带黄棕底色
- 呢喃边框：`1px solid rgba(0,0,0,0.1)` — 几乎看不见
- 白/暖白(#f6f5f4)交替区域
- 4-5 层阴影叠加，单层不透明度不超过 0.05

**组件样式：**

*按钮：*
- 主按钮：bg `#0075de`，白字，圆角 4px，hover 时 bg `#005bab`，active 时 `scale(0.9)`
- 次按钮：bg `rgba(0,0,0,0.05)`，文字 `rgba(0,0,0,0.95)`，圆角 4px，hover 时 `scale(1.05)`
- 药丸标签：bg `#f2f9ff`，文字 `#097fe8`，圆角 9999px，Inter 600 12px

*卡片：*
- 普通卡片：bg `#ffffff`，圆角 12px，`border: 1px solid rgba(0,0,0,0.1)`，4 层阴影(最大不透明度 0.04)
- 暖色卡片：bg `#f6f5f4`，圆角 12px，`border: 1px solid rgba(0,0,0,0.1)`
- 数据卡片：bg `#ffffff`，数据数字用 `rgba(0,0,0,0.95)` + Inter 700 + `font-size: 3rem`

*标签/徽章：*
- 药丸标签：bg `#f2f9ff`，文字 `#097fe8`，圆角 9999px，`letter-spacing: 0.125px`
- 暖灰标签：bg `rgba(0,0,0,0.05)`，文字 `#615d59`，圆角 4px
- 语义标签：绿色(#1aae39)/橙色(#dd5b00)/粉色(#ff64c8) 用于状态

*阴影层级：*
- L1 呢喃：`1px solid rgba(0,0,0,0.1)` — 边框级
- L2 柔和：4 层阴影(最大不透明度 0.04) — 内容卡片
- L3 深度：5 层阴影(最大不透明度 0.05, 52px blur) — 模态/焦点

*装饰线：*
- 分割线：`1px solid rgba(0,0,0,0.1)` — 呢喃级
- 区域分隔：通过白/暖白(#f6f5f4)背景切换实现
- 无硬线分隔 — 一切靠微妙对比

**设计规则：**

*推荐：*
- 使用温暖中性色 — #f6f5f4(暖白)、#615d59(暖灰)、#a39e98(暖浅灰)是 Notion 的灵魂
- 边框永远是呢喃级 1px solid rgba(0,0,0,0.1) — 存在但不打扰
- 白/暖白区域交替创造柔和节奏

*避免：*
- 不要使用冷灰色 — Notion 的灰色都带黄棕底色
- 不要使用超过 0.05 不透明度的单层阴影 — 阴影是"感觉"而非"看到"
- 不要使用纯黑(#000)文字 — 使用 rgba(0,0,0,0.95) 创造微妙温暖

---

## 字体配对速查

| 预设 | 标题字体 | 正文字体 | 来源 |
|------|---------|---------|------|
| Bold Signal | Archivo Black | Space Grotesk | Google |
| Electric Studio | Manrope | Manrope | Google |
| Neon Cyber | Clash Display | Satoshi | Fontshare |
| Dark Botanical | Cormorant | IBM Plex Sans | Google |
| Swiss Modern | Archivo | Nunito | Google |
| Notebook Tabs | Bodoni Moda | DM Sans | Google |
| Pastel Geometry | Plus Jakarta Sans | Plus Jakarta Sans | Google |
| Paper & Ink | Cormorant Garamond | Source Serif 4 | Google |
| Business Blue | Outfit | Outfit | Google |
| Data Dark | JetBrains Mono | JetBrains Mono | Google |
| Gradient Fashion | Syne | Space Mono | Google |
| Terminal Green | JetBrains Mono | JetBrains Mono | Google |
| Apple Minimalist | Sora | Sora | Google |
| Stripe Professional | Work Sans | Work Sans | Google |
| Linear Dark | Inter (wght 510) | Inter | Google |
| Tesla Pure | Figtree | Figtree | Google |
| Vercel Engineering | Geist Sans | Geist Sans | Google |
| Notion Warm | Inter | Inter | Google |
| Ink Garden | Noto Serif SC | Noto Sans SC | Google |

---

## 避免使用的通用模式

**字体：** Inter, Roboto, Arial, 系统字体作为标题
**配色：** `#6366f1`（通用靛蓝），白色背景上的紫色渐变
**布局：** 全部居中，通用英雄区，千篇一律的卡片网格
**装饰：** 写实插画，无意义的毛玻璃，无目的的投影

---

## CSS 注意事项

### 取反CSS函数

**错误 — 浏览器会静默忽略：**
```css
right: -clamp(28px, 3.5vw, 44px);   /* 浏览器忽略 */
```

**正确 — 用 calc() 包裹：**
```css
right: calc(-1 * clamp(28px, 3.5vw, 44px));  /* 生效 */
```
