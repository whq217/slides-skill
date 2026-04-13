<!--
  Slides Skill — HTML 演示文稿模板
  MIT License — © 2025 Zara Zhang (original framework)
                © 2026 Hongqing W (enhanced version)
  https://github.com/zarazhangrui/frontend-slides
-->
# HTML 演示文稿模板

生成幻灯片的参考架构。每个演示文稿遵循此结构。

## 单文件 HTML 基础结构

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{PPT标题}</title>

    <!-- 字体：使用 Google Fonts + 中文回退 -->
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family={字体名}&display=swap">
    <!-- FontAwesome 图标 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

    <!-- Reveal.js CSS — 必须包含，且只加载一次 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.6.1/reveal.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.6.1/theme/black.min.css">

    <!-- ⚠️ 重要：所有视觉覆盖必须放在主题 link 之后的 <style> 中，
         否则会被主题 CSS 覆盖（颜色、字体、背景等） -->
    <style>
        /* ===========================================
           CSS 自定义属性（主题）
           修改这些即可改变整体外观
           =========================================== */
        :root {
            /* 颜色 — 来自选定的主题预设 */
            --bg-primary: #0a0f1c;
            --text-primary: #ffffff;
            --text-secondary: #9ca3af;
            --accent: #00ffcc;
            --accent-glow: rgba(0, 255, 204, 0.3);

            /* 排版 — 使用 rem 单位（不用 vw/vh，Reveal.js 用 transform 缩放固定 1200x700 容器） */
            --font-display: 'Clash Display', 'Noto Sans SC', 'PingFang SC', sans-serif;
            --font-body: 'Satoshi', 'Noto Sans SC', 'PingFang SC', sans-serif;
            --title-size: 2.2rem;
            --subtitle-size: 1.25rem;
            --body-size: 1rem;
            --small-size: 0.75rem;
            --h2-size: 1.1rem;

            /* 动画 */
            --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
            --duration-normal: 0.6s;
        }

        /* --- 在此粘贴 viewport-base.css 的完整内容 --- */

        /* ===========================================
           基础样式 — 全视口背景系统

           关键架构：
           1. 渐变背景挂在 html 上（覆盖整个浏览器视口）
           2. body / .reveal / .slides / section 全部 transparent
           3. 内容页 section 可加微妙半透明底衬提升可读性
           4. 全屏装饰元素（山形/SVG）用 position: fixed 挂在 body

           原因：Reveal.js 用 transform:scale() 缩放 1200×700 容器，
           slide 内元素的背景只覆盖缩放区域，不覆盖整个视口。
           =========================================== */
        * { margin: 0; padding: 0; box-sizing: border-box; }

        /* 全屏渐变背景 — 挂在 html 上确保覆盖整个视口 */
        html {
            background: linear-gradient(180deg, var(--bg-primary) 0%, var(--bg-warm, var(--bg-primary)) 100%) !important;
            min-height: 100vh;
        }

        body {
            font-family: var(--font-body);
            background: transparent !important;
            color: var(--text-primary);
        }

        /* Reveal.js 所有层级设为透明，让 html 渐变透出 */
        .reveal { color: var(--text-primary); background: transparent !important; position: relative; z-index: 1; }
        .reveal .slides { background: transparent !important; }
        .reveal .slides > section { background: transparent !important; }

        /* 强制覆盖主题的标题颜色和字重 */
        .reveal h1, .reveal h2, .reveal h3, .reveal h4 { color: var(--text-primary); }

        /* ⚠️ 强制覆盖 Reveal.js 主题的 text-align:center
           white/black 主题默认给 slide 设 text-align:center，
           不加这条会导致所有卡片内容居中、图标和文字分离。
           必须放在主题 <link> 之后的 <style> 中。 */
        .reveal .slides > section > section:not([data-state="cover"]) {
            text-align: left;
        }

        /* ⚠️ 封面面板也要强制 text-align:left
           即使封面用绝对定位，主题的居中仍会渗透到子元素 */
        .cover-left, .cover-right, .cover-centered, .cover-card {
            text-align: left;
        }
        .cover-centered { text-align: center; }  /* 居中式封面除外 */

        /* ⚠️ 防御性规则：封面和深色背景卡片内文字必须为白色
           原因：上面的 .reveal h1/h3 等选择器优先级高于 .cover-title / .suggest-card h3，
           会把深色背景上的文字覆盖成主文字色（通常是深色），导致看不见。
           必须 !important 确保覆盖。 */
        [data-state="cover"] h1, [data-state="cover"] h2, [data-state="cover"] p,
        [data-state="cover"] span, [data-state="cover"] div:not(.geo-decor) {
            color: #ffffff !important;
        }
        [data-state="cover"] .cover-tag { color: var(--accent) !important; }
        /* 深色背景卡片（建议页、大色块数据页等）内标题也需强制白色 */
        .suggest-card h1, .suggest-card h2, .suggest-card h3, .suggest-card h4 {
            color: #ffffff !important;
        }
        .big-data-card h1, .big-data-card h2, .big-data-card h3, .big-data-card h4,
        .big-data-card .label, .big-data-card p {
            color: #ffffff !important;
        }

        /* ===========================================
           全屏装饰元素 — position: fixed 挂在视口
           装饰 SVG/纹理放在 .reveal 外部，position:fixed
           z-index: 0，低于 .reveal (z:1)
           =========================================== */
        /* .viewport-mountains {
            position: fixed; bottom: 0; left: 0; right: 0;
            height: 120px; pointer-events: none; z-index: 0;
            transition: opacity 0.4s ease;
        }
        body.cover-active .viewport-mountains { opacity: 0; } */

        /* ===========================================
           Reveal.js 幻灯片层级与内边距

           垂直导航模式下 DOM 结构：
             .reveal .slides > section        ← 外层垂直堆叠容器（不可有 padding！）
             .reveal .slides > section > section  ← 实际幻灯片页（有 padding）
           =========================================== */

        /* 外层垂直堆叠容器 — 清除所有内边距 */
        .reveal .slides > section {
            padding: 0 !important;
        }

        /* 实际幻灯片页 — 使用固定 px 内边距 */
        .reveal .slides > section > section {
            padding: 40px 60px 30px !important;
        }

        /* 内容页微妙底衬 — 提升文字可读性（可选，按主题调整透明度） */
        /* .reveal .slides > section > section:not([data-state="cover"]) {
            background: linear-gradient(180deg, rgba(250,248,243,0.55) 0%, rgba(245,240,230,0.4) 100%) !important;
        } */

        /* 非垂直模式（无嵌套 section）的直接子 slide 也加内边距 */
        .reveal .slides > section:not(:has(> section)) {
            padding: 40px 60px 30px !important;
        }

        /* ===========================================
           固定顶部导航栏
           =========================================== */
        .top-nav {
            position: fixed;
            top: 0; left: 0; right: 0;
            height: 48px;
            background: var(--nav-bg, var(--bg-primary));
            color: var(--nav-text, var(--text-primary));
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 30px;
            z-index: 1000;
            font-size: 14px;
        }
        .top-nav .nav-brand {
            font-family: var(--font-display);
            font-weight: 700;
            font-size: 17px;
        }
        .top-nav .nav-links {
            display: flex;
            gap: 20px;
        }
        .top-nav .nav-links a {
            color: rgba(255,255,255,0.6);
            text-decoration: none;
            cursor: pointer;
            transition: color 0.2s;
            position: relative;
        }
        .top-nav .nav-links a:hover,
        .top-nav .nav-links a.active {
            color: var(--text-primary);
        }
        .top-nav .nav-links a.active::after {
            content: '';
            position: absolute;
            bottom: -4px; left: 0; right: 0;
            height: 2px;
            background: var(--accent);
        }

        /* ===========================================
           封面样式 — 必须使用 absolute 定位填满幻灯片
           =========================================== */

        /* 封面 section 绝对定位填满整个 slide 区域 */
        .reveal .slides > section[data-state="cover"],
        .reveal .slides > section > section[data-state="cover"] {
            position: absolute !important;
            top: 0 !important;
            left: 0 !important;
            width: 100% !important;
            height: 100% !important;
            padding: 0 !important;
        }

        /* 样式A — 居中式 */
        .cover-centered {
            display: flex !important;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            width: 100%;
            height: 100%;
            position: relative;
            overflow: hidden;
        }
        .cover-title {
            font-family: var(--font-display);
            font-size: 2.2rem;
            font-weight: 800;
            line-height: 1.2;
            margin-bottom: 0.3em;
        }
        .cover-subtitle {
            font-size: var(--subtitle-size);
            opacity: 0.7;
            margin-bottom: 1.5em;
        }
        .cover-meta {
            font-size: var(--small-size);
            opacity: 0.4;
        }
        .geo-decor {
            position: absolute;
            border-radius: 50%;
            opacity: 0.08;
            background: var(--accent);
        }

        /* 样式B — 分栏式 */
        .cover-split {
            display: flex;
            width: 100%;
            height: 100%;
        }
        .cover-split .cover-left {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding: 60px;
        }
        .cover-split .cover-right {
            flex: 1;
            background: linear-gradient(135deg, var(--accent), var(--bg-primary));
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* 样式C — 卡片式 */
        .cover-card {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 100%;
            height: 100%;
        }
        .cover-card .cover-float-card {
            background: rgba(255,255,255,0.95);
            color: var(--bg-primary);
            border-radius: 20px;
            padding: 50px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.2);
            text-align: center;
            max-width: 700px;
        }

        /* ===========================================
           卡片布局
           =========================================== */
        .card {
            background: var(--card-bg, rgba(255,255,255,0.08));
            border-radius: 6px;
            padding: 14px;
            box-shadow: var(--card-shadow, 0 4px 16px rgba(0,0,0,0.08));
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .card:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 24px rgba(0,0,0,0.15);
        }
        .card-header {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 12px;
        }
        .card-icon {
            width: 26px;
            height: 26px;
            border-radius: 5px;
            background: var(--accent);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 11px;
            flex-shrink: 0;
        }
        /* 防御性：确保标题内 Font Awesome 图标字体不被覆盖 */
        .reveal .slide-title .fas,
        .reveal .slide-title .far,
        .reveal .slide-title .fab,
        .reveal .card-icon .fas,
        .reveal .card-icon .far,
        .reveal .card-icon .fab { font-family: 'Font Awesome 6 Free' !important; font-style: normal !important; font-weight: 900 !important; }
        .reveal .slide-title .fab,
        .reveal .card-icon .fab { font-family: 'Font Awesome 6 Brands' !important; font-weight: 400 !important; }
        .card-title {
            font-family: var(--font-display);
            font-size: 0.82rem;
            font-weight: 500;
        }

        /* ===========================================
           数据放大显示
           =========================================== */
        .data-highlight {
            font-family: var(--font-display);
            font-size: 1.6rem;
            font-weight: 300;
            color: var(--accent);
            line-height: 1.1;
        }
        .data-highlight small {
            font-size: 0.4em;
            font-weight: 600;
            opacity: 0.6;
        }
        .data-label {
            font-size: 0.72rem;
            opacity: 0.5;
            margin-top: 2px;
        }

        /* ===========================================
           多列网格
           =========================================== */
        .card-grid {
            display: grid;
            gap: 12px;
        }
        .card-grid-2 { grid-template-columns: 1fr 1fr; }
        .card-grid-3 { grid-template-columns: 1fr 1fr 1fr; }
        .card-grid-4 { grid-template-columns: 1fr 1fr 1fr 1fr; }

        /* ===========================================
           列表样式
           =========================================== */
        .styled-list {
            list-style: none;
            padding: 0;
        }
        .styled-list li {
            padding: 4px 0 4px 16px;
            position: relative;
            font-size: 0.76rem;
            line-height: 1.5;
        }
        .styled-list li::before {
            content: '';
            position: absolute;
            left: 0;
            top: 9px;
            width: 5px;
            height: 5px;
            border-radius: 50%;
            background: var(--accent);
        }

        /* ===========================================
           图表容器 — 使用固定 px 尺寸
           Reveal.js 用 CSS transform 缩放固定 1200x700 容器，
           vw/vh 引用浏览器视口而非 slide 尺寸，会导致图表错位。
           =========================================== */
        .chart-container {
            width: 100%;
            height: 200px;
        }

        /* ===========================================
           进场动画
           =========================================== */
        .fragment.fade-up {
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.5s var(--ease-out-expo),
                        transform 0.5s var(--ease-out-expo);
        }
        .fragment.fade-up.visible {
            opacity: 1;
            transform: translateY(0);
        }

        /* 页面核心标题 */
        .slide-title {
            font-family: var(--font-display);
            font-size: var(--h2-size);
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 14px;
            padding-bottom: 6px;
            border-bottom: 2px solid var(--accent);
            display: inline-block;
            letter-spacing: -0.2px;
        }

        /* ===========================================
           悬停高亮 — 深色背景页 = 光晕，浅色背景页 = 阴影
           给卡片、数据块等内容模块添加 class="hl"
           =========================================== */
        .reveal .slides section .hl {
            transition: transform 0.3s cubic-bezier(0.16, 1, 0.3, 1),
                        filter 0.3s ease,
                        box-shadow 0.3s ease,
                        border-color 0.3s ease;
        }
        /* 深色背景页：浮起 + 主题色边框 + 主题色双层光晕 */
        .slide-dark .hl:hover {
            transform: translateY(-4px) scale(1.02);
            filter: brightness(1.1);
            border-color: rgba(var(--accent-rgb), 0.55) !important;
            box-shadow: 0 0 15px rgba(var(--accent-rgb), 0.3),
                        0 0 30px rgba(var(--accent-rgb), 0.15);
        }
        /* 浅色背景页：浮起 + 主题色边框 + 暗色投影 */
        .reveal .slides > section > section:not(.slide-dark) .hl:hover {
            transform: translateY(-4px) scale(1.02);
            border-color: rgba(var(--accent-rgb), 0.55) !important;
            box-shadow: 0 8px 32px rgba(0,0,0,0.12);
        }

        /* ===========================================
           页面入场动画（标配）
           内容页：CSS 预隐藏子元素 + JS 带 transition 显示（避免闪烁）
           封面页：@keyframes + .cover-el 类（首屏无 Reveal.js 过渡冲突）
           =========================================== */

        /* 内容页子元素默认隐藏，JS 负责带动画显示 */
        .reveal .slides > section > section:not([data-state="cover"]) > * {
            opacity: 0;
            transform: translateY(20px);
        }

        /* 封面页 @keyframes — 需配合 JS animateCoverSlide() 强制触发
           原因：.present 可能在 CSS 解析前就已存在，导致动画不播放 */
        @keyframes coverContentIn {
            from { opacity: 0; transform: translateY(18px); }
            to { opacity: 1; transform: translateY(0); }
        }
        [data-state="cover"].present .cover-el {
            animation: coverContentIn 0.7s cubic-bezier(0.16,1,0.3,1) both;
        }
        [data-state="cover"].present .cover-el:nth-child(1) { animation-delay: 0.15s; }
        [data-state="cover"].present .cover-el:nth-child(2) { animation-delay: 0.3s; }
        [data-state="cover"].present .cover-el:nth-child(3) { animation-delay: 0.45s; }
        [data-state="cover"].present .cover-el:nth-child(4) { animation-delay: 0.6s; }
        [data-state="cover"].present .cover-el:nth-child(5) { animation-delay: 0.75s; }

        /* 同层级多内容逐个入场动画（stagger，@keyframes方案，不用transition因为Reveal.js会干扰transition的初始值） */
        @keyframes staggerIn {
            from { opacity: 0; transform: translateY(12px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .reveal .slides section .stagger > * {
            opacity: 0;
        }
        .reveal .slides section .stagger.present > * {
            opacity: 1;
            animation: staggerIn 0.6s cubic-bezier(0.25,0.46,0.45,0.94) both;
        }
        .reveal .slides section .stagger.present > *:nth-child(1) { animation-delay: 0.1s; }
        .reveal .slides section .stagger.present > *:nth-child(2) { animation-delay: 0.35s; }
        .reveal .slides section .stagger.present > *:nth-child(3) { animation-delay: 0.6s; }
        .reveal .slides section .stagger.present > *:nth-child(4) { animation-delay: 0.85s; }
        .reveal .slides section .stagger.present > *:nth-child(5) { animation-delay: 1.1s; }
        .reveal .slides section .stagger.present > *:nth-child(6) { animation-delay: 1.35s; }

        /* ===========================================
           画笔工具
           =========================================== */
        .pen-canvas {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            z-index: 500;
            pointer-events: none;
        }
        .pen-canvas.active {
            pointer-events: auto;
            cursor: crosshair;
        }
        .pen-toggle {
            position: fixed;
            bottom: 20px; right: 20px;
            z-index: 1000;
            width: 44px; height: 44px;
            border-radius: 50%;
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            color: #fff;
            font-size: 16px;
            cursor: pointer;
            opacity: 0.4;
            transition: all 0.3s;
        }
        .pen-toggle:hover {
            opacity: 1;
            background: rgba(255,255,255,0.2);
        }
        .pen-toggle.active {
            opacity: 1;
            background: var(--accent, #ff6b6b);
            color: #000;
            border-color: transparent;
            box-shadow: 0 0 16px rgba(255,107,107,0.4);
        }
    </style>
</head>
<body>

    <!-- 全屏装饰元素（按需启用）— position:fixed 在视口底部 -->
    <!-- <div class="viewport-mountains">
        <svg viewBox="0 0 1200 120" preserveAspectRatio="none" width="100%" height="100%">
            <path d="M0,120 L0,85 Q100,50 200,72 Q300,90 400,58 Q500,30 600,68 Q700,90 800,50 Q900,18 1000,62 Q1100,85 1200,45 L1200,120 Z" fill="rgba(44,36,24,0.04)"/>
            <path d="M0,120 L0,95 Q120,65 240,88 Q360,100 480,72 Q600,48 720,82 Q840,100 960,65 Q1060,45 1200,78 L1200,120 Z" fill="rgba(44,36,24,0.07)"/>
        </svg>
    </div> -->

    <!-- 固定顶部导航栏 -->
    <nav class="top-nav">
        <div class="nav-brand">{标题简称}</div>
        <div class="nav-links">
            <a href="#" data-slide="0">章节1</a>
            <a href="#" data-slide="3">章节2</a>
        </div>
    </nav>

    <!-- 幻灯片内容 — 使用垂直导航：外层 section 包裹一组垂直 slide -->
    <div class="reveal">
        <div class="slides">

            <!-- ========== 第一组（垂直堆叠）========== -->
            <!-- 外层 section 是垂直导航的容器，不含任何内容 -->
            <section>

                <!-- 第1页：封面 — 以下是页面内容，可直接编辑 -->
                <section data-state="cover" class="cover-centered">
                    <h1 class="cover-title">{标题}</h1>
                    <p class="cover-subtitle">{副标题}</p>
                </section>

                <!-- 第2页：{标题} — 以下是页面内容，可直接编辑 -->
                <section>
                    <h2 class="slide-title">{页面标题}</h2>
                    <div class="card-grid card-grid-2">
                        <div class="card fragment fade-up">
                            <div class="card-header">
                                <div class="card-icon"><i class="fas fa-chart-line"></i></div>
                                <div class="card-title">卡片标题</div>
                            </div>
                            <div class="data-highlight">128%</div>
                            <div class="data-label">说明文字</div>
                        </div>
                    </div>
                </section>

                <!-- 第3页：带图表的页面 — 图表容器使用固定 px 尺寸 -->
                <section>
                    <h2 class="slide-title">数据概览</h2>
                    <!-- 图表容器：必须用固定 px，不要用 vw/vh -->
                    <div id="chart1" class="chart-container"></div>
                </section>

            </section>

            <!-- ========== 第二组（垂直堆叠）========== -->
            <section>

                <!-- 第4页 -->
                <section>
                    <h2 class="slide-title">另一章节</h2>
                    <p>页面内容</p>
                </section>

            </section>

        </div>
    </div>

    <!-- Reveal.js 核心 JS -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/reveal.js/4.6.1/reveal.min.js"></script>

    <!-- ECharts — 仅数据驱动型演示文稿包含 -->
    <!-- 以下是图表依赖 -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/echarts/5.5.0/echarts.min.js"></script>

    <script>
    // ========== Reveal.js 初始化 ==========
    // 垂直导航：键盘上下键在垂直堆叠的 slide 间切换
    // 进度条：底部显示，不显示页码
    Reveal.initialize({
        hash: true,
        transition: 'slide',
        transitionSpeed: 'default',
        width: 1200,
        height: 700,
        margin: 0.04,
        controls: false,           // 隐藏翻页箭头（已有键盘/滚轮/点击翻页）
        controlsTutorial: false,
        progress: true,         // 底部进度条
        center: false,
        slideNumber: false,     // 不显示页码
        keyboard: true,         // 启用键盘导航（上下左右）
        mouseWheel: true,       // 启用鼠标滚轮翻页
        navigationMode: 'default'  // 支持垂直导航
    });

    // ========== 顶部导航栏 ==========
    // ⚠️ 垂直导航注意：所有 slide 在同一个外层 section 的垂直栈中，
    // Reveal.slide(indexh, indexv, indexf) 必须传两个参数！
    // indexh=0（唯一的水平组），indexv=data-slide 值（垂直位置）
    const navLinks = document.querySelectorAll('.top-nav .nav-links a');
    navLinks.forEach(link => {
        link.addEventListener('click', function(e) {
            e.preventDefault();
            var idx = parseInt(this.getAttribute('data-slide'));
            Reveal.slide(0, idx);  // 第一个参数固定为 0
        });
    });
    // ⚠️ 垂直导航下用 event.indexv，不是 event.indexh
    Reveal.on('slidechanged', function(event) {
        let v = event.indexv;  // 垂直索引
        let activeLink = null;
        navLinks.forEach(link => {
            const target = parseInt(link.getAttribute('data-slide'));
            link.classList.remove('active');
            if (v >= target) activeLink = link;
        });
        navLinks.forEach(l => l.classList.remove('active'));
        if (activeLink) activeLink.classList.add('active');
    });
    // ready 时也初始化高亮
    Reveal.on('ready', function() {
        let v = Reveal.getIndices().v;
        let activeLink = null;
        navLinks.forEach(link => {
            const target = parseInt(link.getAttribute('data-slide'));
            link.classList.remove('active');
            if (v >= target) activeLink = link;
        });
        navLinks.forEach(l => l.classList.remove('active'));
        if (activeLink) activeLink.classList.add('active');
    });

    // ========== ECharts 图表 ==========
    // 以下是图表数据，可修改
    const chartConfigs = {};
    // 已初始化的图表实例缓存，避免重复创建
    const chartInstances = {};

    function renderCharts() {
        Object.entries(chartConfigs).forEach(([id, config]) => {
            const dom = document.getElementById(id);
            if (!dom) return;
            // 复用已有实例，避免重复初始化
            if (!chartInstances[id]) {
                chartInstances[id] = echarts.init(dom);
            }
            chartInstances[id].setOption(config);
        });
    }

    // 初始渲染
    Reveal.on('ready', renderCharts);

    // 关键：slide 切换时重新 resize 图表
    // ECharts 只能在可见的 DOM 元素上正确渲染，
    // slide 切换后之前隐藏的图表变为可见，必须触发 resize
    Reveal.on('slidechanged', function() {
        // 等待 Reveal.js 完成 slide 过渡动画
        setTimeout(function() {
            Object.values(chartInstances).forEach(function(chart) {
                chart.resize();
            });
        }, 300);
    });

    // 浏览器窗口 resize 时也更新图表
    window.addEventListener('resize', function() {
        Object.values(chartInstances).forEach(function(chart) {
            chart.resize();
        });
    });

    // ========== 页面入场动画 ==========
    // 内容页：CSS 已预设子元素 opacity:0 + translateY(20px)
    // JS 在 Reveal.js slide 过渡结束后（400ms），带 transition 显示子元素
    // 优先级：页面元素动画 → 图表入场动画（仅当页有图表时触发）
    function animateContentSlide() {
        var current = Reveal.getCurrentSlide();
        if (!current || current.getAttribute('data-state') === 'cover') return;
        // Stagger: 同层级容器内子元素依次入场（@keyframes驱动）
        current.querySelectorAll('.stagger').forEach(function(container) {
            container.classList.remove('present');
            void container.offsetHeight;
            container.classList.add('present');
        });
        // 非 stagger 容器的直接子元素：淡入
        var els = Array.prototype.slice.call(current.children);
        els.forEach(function(el, i) {
            if (el.classList.contains('fragment') || el.classList.contains('stagger')) return;
            el.style.transition = 'opacity 0.6s cubic-bezier(0.25,0.46,0.45,0.94), transform 0.6s cubic-bezier(0.25,0.46,0.45,0.94)';
            el.style.transitionDelay = (i * 0.1) + 's';
            el.style.opacity = '1';
            el.style.transform = 'translateY(0)';
        });
        // 检测当前页是否有图表容器 → 有则在页面元素动画完成后触发图表动画
        if (current.querySelector('[id^="chart"]')) {
            setTimeout(animateChartsInSlide, 500);
        }
        // 排行榜进度条动画（使用 data-w 属性）
        var rankBars = current.querySelectorAll('.rank-fill[data-w]');
        if (rankBars.length) {
            rankBars.forEach(function(bar) {
                bar.style.transition = 'none';
                bar.style.width = '0%';
            });
            setTimeout(function() {
                rankBars.forEach(function(bar, i) {
                    bar.style.transition = 'width 0.8s cubic-bezier(0.16, 1, 0.3, 1)';
                    setTimeout(function() { bar.style.width = bar.getAttribute('data-w'); }, i * 150);
                });
            }, 500);
        }
    }

    // ========== 图表入场动画 ==========
    // ECharts 内置动画：clear() → setOption() 触发入场动画
    // 按图表类型匹配不同动画节奏（折线/饼图/柱图/雷达图）
    var chartAnimPresets = {
        line:    { animationDuration: 1200, animationEasing: 'cubicInOut' },
        pie:     { animationDuration: 1000, animationEasing: 'cubicOut', animationDelay: function(i){ return i * 120; } },
        bar:     { animationDuration: 900,  animationEasing: 'cubicOut', animationDelay: function(i){ return i * 150; } },
        radar:   { animationDuration: 1000, animationEasing: 'cubicOut' },
        default: { animationDuration: 1000, animationEasing: 'cubicOut' }
    };
    function getChartAnimOpts(config) {
        var preset = chartAnimPresets.default;
        if (config.series && config.series[0]) {
            preset = chartAnimPresets[config.series[0].type] || chartAnimPresets.default;
        }
        return { animation: true, animationDuration: preset.animationDuration, animationEasing: preset.animationEasing, animationDelay: preset.animationDelay };
    }
    // 在当前 slide 中查找图表并重播动画
    function animateChartsInSlide() {
        var current = Reveal.getCurrentSlide();
        if (!current) return;
        var chartDoms = current.querySelectorAll('[id^="chart"]');
        if (chartDoms.length === 0) return;
        chartDoms.forEach(function(dom) {
            var id = dom.id;
            if (!chartInstances[id] || !chartConfigs[id]) return;
            var animOpts = getChartAnimOpts(chartConfigs[id]);
            var mergedConfig = Object.assign({}, chartConfigs[id], animOpts);
            chartInstances[id].clear();
            chartInstances[id].setOption(mergedConfig);
        });
    }

    // 离开页面时：清除内联样式，CSS 规则重新接管隐藏（transition:none 确保不播动画）
    function resetSlideAnim(event) {
        var prev = event.previousSlide;
        if (prev && prev.getAttribute('data-state') !== 'cover') {
            Array.prototype.slice.call(prev.children).forEach(function(el) {
                if (el.classList.contains('stagger')) return;
                el.style.transition = 'none';
                el.style.opacity = '';
                el.style.transform = '';
                el.style.transitionDelay = '';
            });
            // 重置 stagger 容器
            prev.querySelectorAll('.stagger').forEach(function(c) { c.classList.remove('present'); });
            // 重置排行榜进度条
            prev.querySelectorAll('.rank-fill').forEach(function(bar) {
                bar.style.transition = 'none';
                bar.style.width = '0%';
            });
        }
    }

    // ========== 封面动画 JS 触发（必须！CSS @keyframes 可能因竞态不触发） ==========
    function updateCoverState() {
        var current = Reveal.getCurrentSlide();
        // 配合 body.cover-active 控制 page-specific 元素显隐
        if (current && current.getAttribute('data-state') === 'cover') {
            document.body.classList.add('cover-active');
        } else {
            document.body.classList.remove('cover-active');
        }
    }
    function animateCoverSlide() {
        var cover = document.querySelector('[data-state="cover"]');
        if (!cover) return;
        var els = cover.querySelectorAll('.cover-el');
        els.forEach(function(el) {
            el.style.animation = 'none';
            void el.offsetHeight; // 强制重排
            el.style.animation = '';
        });
    }
    Reveal.on('ready', function() {
        updateCoverState();
        setTimeout(animateCoverSlide, 100);
        setTimeout(animateContentSlide, 400);
    });
    Reveal.on('slidechanged', function(e) {
        resetSlideAnim(e);
        updateCoverState();
        var current = Reveal.getCurrentSlide();
        if (current && current.getAttribute('data-state') === 'cover') {
            setTimeout(animateCoverSlide, 100);
        }
        setTimeout(animateContentSlide, 400);
    });

    // ========== 浏览器内编辑（仅用户选择开启时包含） ==========
    {INLINE_EDIT_CODE}
    </script>
</body>
</html>
```

---

## 浏览器内编辑实现（用户选择开启时才包含）

**使用 JS 实现，而非 CSS 兄弟选择器（会失败）。**

```html
<!-- 编辑模式热区 -->
<div class="edit-hotzone"></div>
<button class="edit-toggle" id="editToggle" title="编辑模式 (E)">Edit</button>

<!-- 画笔工具 -->
<canvas class="pen-canvas" id="penCanvas"></canvas>
<button class="pen-toggle" id="penToggle" title="画笔模式 (D)">
    <i class="fas fa-pen"></i>
</button>
```

```css
.edit-hotzone {
    position: fixed; top: 0; left: 0;
    width: 80px; height: 80px;
    z-index: 10000;
    cursor: pointer;
}
.edit-toggle {
    position: fixed; top: 10px; left: 10px;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.3s ease;
    z-index: 10001;
    background: var(--accent);
    color: white;
    border: none;
    border-radius: 8px;
    padding: 8px 14px;
    cursor: pointer;
    font-size: 16px;
}
.edit-toggle.show,
.edit-toggle.active {
    opacity: 1;
    pointer-events: auto;
}
.edit-mode .reveal .slides section [contenteditable="true"] {
    outline: 1px dashed rgba(251,191,36,0.4);
    outline-offset: 2px;
    cursor: text;
    min-height: 1em;
}
.edit-mode .reveal .slides section [contenteditable="true"]:hover {
    outline-color: var(--accent);
    background: rgba(251,191,36,0.05);
}
```

```javascript
// 编辑控制器
const editToggle = document.getElementById('editToggle');
const hotzone = document.querySelector('.edit-hotzone');
let isEditMode = false;
let hideTimeout = null;

function toggleEditMode() {
    isEditMode = !isEditMode;
    document.body.classList.toggle('edit-mode', isEditMode);
    editToggle.classList.toggle('active', isEditMode);

    // 编辑模式下，slide 内所有文本元素均可编辑
    // 排除：容器级div（有子div的）、导航栏、按钮、script/style
    document.querySelectorAll('.reveal .slides section').forEach(sec => {
        sec.querySelectorAll('div, p, span, li, h1, h2, h3, h4, strong, em, i, a').forEach(el => {
            // 跳过含有块级子元素的容器（避免误编辑结构）
            if (!el.querySelector('div, section, nav')) {
                el.contentEditable = isEditMode ? 'true' : 'false';
            }
        });
    });

    if (isEditMode) {
        editToggle.textContent = 'Save';
        editToggle.title = '保存并退出编辑 (E)';
    } else {
        editToggle.textContent = 'Edit';
        editToggle.title = '编辑模式 (E)';
        // 自动保存到 localStorage
        localStorage.setItem('slides-content', document.querySelector('.reveal').innerHTML);
    }
}

editToggle.addEventListener('click', toggleEditMode);

// 热区悬停 + 400ms 宽限期
hotzone.addEventListener('mouseenter', () => {
    clearTimeout(hideTimeout);
    editToggle.classList.add('show');
});
hotzone.addEventListener('mouseleave', () => {
    hideTimeout = setTimeout(() => {
        if (!isEditMode) editToggle.classList.remove('show');
    }, 400);
});
editToggle.addEventListener('mouseenter', () => clearTimeout(hideTimeout));
editToggle.addEventListener('mouseleave', () => {
    hideTimeout = setTimeout(() => {
        if (!isEditMode) editToggle.classList.remove('show');
    }, 400);
});
hotzone.addEventListener('click', toggleEditMode);

// E 键切换
document.addEventListener('keydown', (e) => {
    if ((e.key === 'e' || e.key === 'E') && !e.target.getAttribute('contenteditable')) {
        toggleEditMode();
    }
});

// 点击空白处翻页：先逐步展示fragment，全部展示完再翻页
// 逻辑：排除编辑模式、交互元素(canvas/a/button)、含直接文本的元素
// 其余视为空白区域，调用 Reveal.next() 自动处理 fragment→翻页
document.querySelector('.reveal').addEventListener('click', (e) => {
    if (document.body.classList.contains('edit-mode')) return;
    const el = e.target;
    if (['CANVAS','A','BUTTON','INPUT','TEXTAREA','SELECT'].includes(el.tagName)) return;
    if (el.contentEditable === 'true') return;
    // 检查是否包含直接文本（非子元素文本）
    const hasText = Array.from(el.childNodes).some(n => n.nodeType === 3 && n.textContent.trim());
    if (hasText) return;
    Reveal.next();
});

// 右键点击空白处向前翻页（画笔模式下右键用于清除标注，不翻页）
document.querySelector('.reveal').addEventListener('contextmenu', (e) => {
    if (document.getElementById('penCanvas').classList.contains('active')) return;
    e.preventDefault();
    const el = e.target;
    if (['CANVAS','A','BUTTON','INPUT','TEXTAREA','SELECT'].includes(el.tagName)) return;
    const hasText = Array.from(el.childNodes).some(n => n.nodeType === 3 && n.textContent.trim());
    if (hasText) return;
    Reveal.prev();
});
// 注意：如需直接跳到上一页（不逐步收fragment），改用：
// var idx = Reveal.getIndices();
// if (idx.v > 0) Reveal.slide(idx.h, idx.v - 1, 99);
// else if (idx.h > 0) Reveal.slide(idx.h - 1, 99, 99);

// ========== 画笔工具 ==========
const penCanvas = document.getElementById('penCanvas');
const penCtx = penCanvas.getContext('2d');
const penToggle = document.getElementById('penToggle');
let isPenMode = false;
let isDrawing = false;

function resizePenCanvas() {
    penCanvas.width = window.innerWidth;
    penCanvas.height = window.innerHeight;
}
resizePenCanvas();
window.addEventListener('resize', resizePenCanvas);

function togglePen() {
    isPenMode = !isPenMode;
    penCanvas.classList.toggle('active', isPenMode);
    penToggle.classList.toggle('active', isPenMode);
}
penToggle.addEventListener('click', togglePen);

// D 键切换画笔
document.addEventListener('keydown', (e) => {
    if ((e.key === 'd' || e.key === 'D') && !e.target.getAttribute('contenteditable')) {
        togglePen();
    }
});

// 绘画（每次 mousedown 重新设置属性，避免 resize 后丢失）
penCanvas.addEventListener('mousedown', (e) => {
    if (e.button === 2) return;
    isDrawing = true;
    penCtx.lineWidth = 3;
    penCtx.lineCap = 'round';
    penCtx.lineJoin = 'round';
    penCtx.strokeStyle = 'rgba(255, 50, 50, 0.9)';
    penCtx.beginPath();
    penCtx.moveTo(e.clientX, e.clientY);
});
penCanvas.addEventListener('mousemove', (e) => {
    if (!isDrawing) return;
    penCtx.lineTo(e.clientX, e.clientY);
    penCtx.stroke();
});
penCanvas.addEventListener('mouseup', () => { isDrawing = false; });
penCanvas.addEventListener('mouseleave', () => { isDrawing = false; });

// 右键清除画笔标注
penCanvas.addEventListener('contextmenu', (e) => {
    e.preventDefault();
    penCtx.clearRect(0, 0, penCanvas.width, penCanvas.height);
});

// 翻页自动清除画笔
Reveal.on('slidechanged', () => {
    penCtx.clearRect(0, 0, penCanvas.width, penCanvas.height);
});

// 注意：禁止 localStorage 自动恢复！
// 编辑保存功能保留（用户点 Save 时存入 localStorage），
// 但绝不自动加载旧内容覆盖新生成的 HTML。
// 原因：每次重新生成 PPT 后，旧版本 localStorage 内容会覆盖新版本。
```

---

## ECharts 条件加载策略

| 演示文稿类型 | ECharts 处理 |
|---|---|
| 文字主导型（无图表） | 不包含 ECharts，文件更小 |
| 数据驱动型（有图表） | CDN `<script>` 加载 |
| 平衡型（1-2个图表） | CDN `<script>` 加载，离线时显示优雅降级提示 |

---

## 关键技术要点

### Reveal.js 缩放机制
Reveal.js 使用 CSS transform 将固定 1200x700 容器缩放到浏览器视口大小。因此：
- **字体大小**：使用 `rem` 或固定 `px`，不要用 `vw`/`vh`
- **图表容器**：使用固定 `px` 尺寸（如 `width: 100%; height: 200px`）
- **内边距**：使用固定 `px` 值
- **全屏背景**：必须挂在 `html`/`body` 或 `position: fixed` 元素上，不能挂在 slide 内元素上
- **全屏装饰**：SVG/纹理等用 `position: fixed` + `z-index: 0`，确保覆盖整个视口

### 全屏背景架构（必读）

```
视口背景层次（从底到顶）：
┌─────────────────────────────────────┐
│ html          — 渐变背景（全视口）    │
│ body          — transparent          │
│ .viewport-*  — fixed 装饰（z:0）    │
│ .reveal      — transparent（z:1）    │
│ section      — 内容（可加微妙底衬）  │
│ .top-nav     — 固定导航（z:1000）    │
│ .pen-canvas  — 画笔层（z:500）       │
│ .edit-*      — 编辑UI（z:10000+）   │
└─────────────────────────────────────┘
```

**为什么不能把背景写在 slide 内？** Reveal.js 用 `transform: scale()` 缩放 1200×700 容器，slide 内元素的背景只覆盖缩放区域，不覆盖整个浏览器视口。在宽屏/非 16:9 显示器上会出现白色边框。

### 主题 CSS 优先级防御
Reveal.js 主题（如 `white.min.css`、`black.min.css`）对颜色/字体/背景有完整声明。防御策略：
1. 所有视觉覆盖放在主题 `<link>` **之后**的 `<style>` 中
2. 关键颜色属性用 `!important` 确保不被覆盖
3. 每次切换主题后必须回归检查字体颜色和背景

### 封面动画双轨保障
封面动画不能纯依赖 CSS `@keyframes`——`.present` 类的添加时机与 CSS 解析存在竞态：
- **CSS 轨**：`@keyframes coverContentIn` + `.cover-el` 类提供视觉定义
- **JS 轨**：`animateCoverSlide()` 在 `ready` 和 `slidechanged` 事件中强制重排重播
- 两者配合确保封面动画 100% 可靠触发

### 动画属性禁止 !important
需要被 JS 动态修改的属性（`width`、`opacity`、`transform`、`height`），CSS 中绝对不能加 `!important`。
`!important` 的优先级高于 JS 内联 `element.style`，会导致动画无法触发。

### 图文布局图片显示规则（必读）
在 Reveal.js 的 `transform: scale()` 缩放环境下，`<img>` 标签的尺寸解析不可靠：
- flex/grid 推导的父容器高度不是 CSS 规范中的"确定高度"
- `<img>` 的 `height: 100%` 无法可靠解析，`object-fit: cover` 配合 `position: absolute` 也存在对齐问题

**必须使用 `background-image` 方案：**
```html
<!-- ✅ 正确：background-image 方案 -->
<div class="bg-img" style="background-image:url('图片URL')"></div>
```
```css
.bg-img {
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
}
```
```html
<!-- ❌ 错误：img 标签方案 — 不可靠 -->
<div class="img-container">
    <img src="图片URL" style="width:100%; height:100%; object-fit:cover;">
</div>
```

**适用范围：** 所有图文卡片布局（IC-1/IC-2/IC-3/IC-4A/IC-4B/IC-4C）、全出血大图（H-1）、封面背景图（C-3）。
**例外：** H-2 左右交替图文中的插图可用 `<img>` 标签（非全填充场景）。

### 垂直导航 DOM 结构
```
.reveal .slides
  └─ <section>                  ← 外层垂直堆叠容器（padding: 0）
       ├─ <section> slide 1     ← 实际幻灯片（padding: 40px 60px 30px）
       ├─ <section> slide 2
       └─ <section> slide 3
  └─ <section>                  ← 第二组垂直堆叠
       └─ <section> slide 4
```

### 图表渲染时序
1. `Reveal.on('ready')` — 初始渲染可见 slide 上的图表
2. `Reveal.on('slidechanged')` — slide 切换后 300ms 延迟 resize 图表
3. `window.addEventListener('resize')` — 浏览器窗口变化时 resize

### 代码质量

- 语义化 HTML（`<section>`, `<nav>`）
- 键盘导航完整可用（上下左右方向键）
- `prefers-reduced-motion` 支持（viewport-base.css 已包含）
- `<!-- 固定顶部导航栏 -->` — 导航栏区域
- `// ========== 功能模块名 ==========` — JS 模块分隔
