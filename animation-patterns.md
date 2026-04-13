<!--
  Slides Skill — 动画模式参考
  MIT License — © 2025 Zara Zhang (original framework)
                © 2026 Hongqing W (enhanced version)
  https://github.com/zarazhangrui/frontend-slides
-->
# 动画模式参考（可选）

> **默认入场动画已内置在 html-template.md 中**，无需额外操作。
> 内容页：CSS 预隐藏子元素 + JS 带 transition 显示。封面页：@keyframes + `.cover-el` 类。
> 此文件提供额外的动画效果选项（封面装饰、背景效果、特殊过渡），当用户要求更丰富的视觉效果时读取。

---

## 默认入场动画原理（已在 html-template.md 中内置）

### 为什么不能只用 CSS transition？

Reveal.js 翻页时有自己的 slide 过渡动画（~400ms）。如果内容页子元素的 transition 在 `.present` 添加时立即触发，会和 Reveal.js 过渡重叠，导致"一闪而过"或"先出现再闪"。

### 双轨方案

| | 封面页 | 内容页 |
|---|---|---|
| **触发方式** | CSS `@keyframes` + `.present` | JS 内联样式 + `setTimeout` |
| **原理** | 首屏加载时 `.present` 已存在，`@keyframes` 自动播放 | CSS 预设 `opacity:0`，JS 在 Reveal.js 过渡结束后（400ms）带 transition 设为 `opacity:1` |
| **元素标记** | 需要 `class="cover-el"` | 无需额外 class，`> *` 选择器自动命中直接子元素 |

### 关键代码片段

**CSS — 内容页预隐藏：**
```css
.reveal .slides > section > section:not([data-state="cover"]) > * {
    opacity: 0;
    transform: translateY(20px);
}
```

**CSS — 封面 @keyframes：**
```css
@keyframes coverContentIn {
    from { opacity: 0; transform: translateY(18px); }
    to { opacity: 1; transform: translateY(0); }
}
[data-state="cover"].present .cover-el {
    animation: coverContentIn 0.7s cubic-bezier(0.16,1,0.3,1) both;
}
```

**JS — 内容页动画触发：**
```javascript
function animateContentSlide() {
    var current = Reveal.getCurrentSlide();
    if (!current || current.getAttribute('data-state') === 'cover') return;
    var els = Array.prototype.slice.call(current.children);
    els.forEach(function(el, i) {
        el.style.transition = 'opacity 0.6s ease, transform 0.6s ease';
        el.style.transitionDelay = (i * 0.1) + 's';
        el.style.opacity = '1';
        el.style.transform = 'translateY(0)';
    });
}
function resetSlideAnim(event) {
    var prev = event.previousSlide;
    if (prev && prev.getAttribute('data-state') !== 'cover') {
        Array.prototype.slice.call(prev.children).forEach(function(el) {
            el.style.transition = 'none';
            el.style.opacity = '';
            el.style.transform = '';
            el.style.transitionDelay = '';
        });
    }
}
Reveal.on('ready', function() { setTimeout(animateContentSlide, 400); });
Reveal.on('slidechanged', function(e) { resetSlideAnim(e); setTimeout(animateContentSlide, 400); });
```

### 可调参数

| 参数 | 默认值 | 作用 |
|------|--------|------|
| 动画延迟 | 400ms | 等 Reveal.js slide 过渡结束，值太小会重叠闪烁 |
| 过渡时长 | 0.6s | 子元素淡入持续时间 |
| 浮入距离 | 20px | `translateY` 位移，越大越明显 |
| 交错间隔 | 0.1s | 每个子元素间的延迟，越大越"逐个出现" |

## 效果→氛围映射

与 SKILL.md 5.0 氛围选项（A-D）一一对应，氛围选择后自动匹配动画风格。

| 氛围 | 动画风格 | 具体参数 | 视觉线索 |
|------|---------|---------|----------|
| **A. 震撼/自信** | 慢淡入+大缩放 | 淡入1-1.5s，缩放0.9→1，序列延迟0.15s | 暗色背景，聚光灯效果，强对比 |
| **B. 兴奋/活力** | 弹性缓动+霓虹发光 | 弹性缓动cubic-bezier，box-shadow发光，浮动/摇摆 | 鲜明色彩，圆角，动态感强 |
| **C. 平静/专注** | 柔和淡入+微妙运动 | 非常慢淡入0.8-1.2s，translateY仅10-15px | 大量留白，柔和色调，精确间距 |
| **D. 温暖/感动** | 温暖渐变+缓慢揭示 | 柔和缩放1.02→1，暖色调overlay渐现 | 暖色系，有机形状，温度感 |

---

## 封面页动画（重点）

封面是PPT的第一印象，需要视觉冲击力。以下效果可组合使用。

### 1. 标题入场动画（三选一）

```css
/* A. 淡入上滑 + 序列延迟（推荐，最通用） */
.cover-title, .cover-subtitle, .cover-meta {
    opacity: 0;
    transform: translateY(30px);
}
/* ⚠️ 关键：.present 由 Reveal.js 添加到 section 上，
   如果封面class也在section上（如 class="cover-centered"），
   必须用 .cover-centered.present 组合选择器，而非 .present .cover-title 后代选择器 */
.cover-centered.present .cover-title {
    opacity: 1; transform: translateY(0);
    transition: opacity 0.8s cubic-bezier(0.16,1,0.3,1), transform 0.8s cubic-bezier(0.16,1,0.3,1);
}
.cover-centered.present .cover-subtitle {
    opacity: 1; transform: translateY(0);
    transition: opacity 0.8s cubic-bezier(0.16,1,0.3,1) 0.2s, transform 0.8s cubic-bezier(0.16,1,0.3,1) 0.2s;
}
.cover-centered.present .cover-meta {
    opacity: 1; transform: translateY(0);
    transition: opacity 0.8s cubic-bezier(0.16,1,0.3,1) 0.4s, transform 0.8s cubic-bezier(0.16,1,0.3,1) 0.4s;
}

/* B. 缩放弹入（适合科技/未来主题） */
.cover-title {
    opacity: 0;
    transform: scale(0.8);
    transition: opacity 0.6s, transform 0.6s cubic-bezier(0.16,1,0.3,1);
}
.cover-centered.present .cover-title {
    opacity: 1; transform: scale(1);
}

/* C. 模糊到清晰（适合平静/极简主题） */
.cover-title {
    opacity: 0;
    filter: blur(12px);
    transition: opacity 1s, filter 1s cubic-bezier(0.16,1,0.3,1);
}
.cover-centered.present .cover-title {
    opacity: 1; filter: blur(0);
}
```

> **Reveal.js 提示**：封面 `<section>` 打开时 Reveal.js 会在该元素上添加 `.present` 类。
> 由于封面class（如 `.cover-centered`）也在 `<section>` 上，必须使用 **组合选择器** `.cover-centered.present` 而非后代选择器 `.present .cover-centered`。

### 2. 封面背景效果（叠加在封面上）

```css
/* A. 渐变光晕 — 两个模糊圆形，制造深度 */
.cover-glow::before,
.cover-glow::after {
    content: '';
    position: absolute;
    border-radius: 50%;
    pointer-events: none;
}
.cover-glow::before {
    width: 400px; height: 400px;
    background: radial-gradient(circle, rgba(var(--grad1-rgb,255,107,107),0.2), transparent 70%);
    top: -80px; left: -80px;
}
.cover-glow::after {
    width: 350px; height: 350px;
    background: radial-gradient(circle, rgba(var(--grad3-rgb,72,219,251),0.15), transparent 70%);
    bottom: -60px; right: -60px;
}

/* B. 浮动几何装饰 — 缓慢漂移的圆形 */
.geo-float {
    position: absolute;
    border-radius: 50%;
    opacity: 0.08;
    background: var(--accent);
    filter: blur(40px);
    animation: geoFloat 20s infinite ease-in-out;
}
@keyframes geoFloat {
    0%, 100% { transform: translate(0, 0) scale(1); }
    33%      { transform: translate(30px, -30px) scale(1.1); }
    66%      { transform: translate(-20px, 20px) scale(0.9); }
}

/* C. 网格图案 — 科技感 */
.cover-grid {
    background-image:
        linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
    background-size: 50px 50px;
}

/* D. 动态渐变背景 — 缓慢移动的色彩 */
.cover-gradient-shift {
    background-size: 200% 200%;
    animation: gradientShift 15s ease infinite;
}
@keyframes gradientShift {
    0%, 100% { background-position: 0% 50%; }
    50%      { background-position: 100% 50%; }
}

/* E. 旋转光斑 — 分栏式封面右侧 */
.cover-split .cover-right::before {
    content: '';
    position: absolute;
    top: -50%; left: -50%;
    width: 200%; height: 200%;
    background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
    animation: rotateGlow 30s linear infinite;
}
@keyframes rotateGlow {
    from { transform: rotate(0deg); }
    to   { transform: rotate(360deg); }
}

/* F. 卡片悬浮 — 卡片式封面 */
.cover-float-card {
    animation: floatCard 6s ease-in-out infinite;
}
@keyframes floatCard {
    0%, 100% { transform: translateY(0) scale(1); }
    50%      { transform: translateY(-10px) scale(1.02); }
}
```

### 3. 封面装饰线条

```css
/* 渐变分割线 — 标题与副标题之间 */
.cover-divider {
    width: 80px; height: 3px;
    background: linear-gradient(90deg, var(--grad1, #ff6b6b), var(--grad2, #c471f5), var(--grad3, #48dbfb));
    border-radius: 2px;
    margin: 16px auto;
}

/* 标签徽章 — 居中或卡片式封面 */
.cover-badge {
    display: inline-block;
    padding: 4px 18px;
    border-radius: 30px;
    font-size: 11px;
    letter-spacing: 3px;
    font-weight: 700;
    margin-bottom: 20px;
}
```

### 4. 封面完整示例

```html
<!-- 居中式封面 — 渐变光晕 + 浮动装饰 + 序列入场 -->
<section data-state="cover" class="cover-centered cover-glow">
    <div class="geo-float" style="width:400px;height:400px;top:-200px;right:-200px"></div>
    <div class="geo-float" style="width:300px;height:300px;bottom:-150px;left:-150px"></div>
    <div class="cover-badge" style="background:var(--accent);color:#000">2026 市场分析</div>
    <h1 class="cover-title">AI<strong>漫剧</strong>市场报告</h1>
    <div class="cover-divider"></div>
    <p class="cover-subtitle">市场规模 · 产业链 · 竞争格局</p>
    <p class="cover-meta">数据来源：艾媒咨询 / 36氪</p>
</section>
```

---

## 内页入场动画

```css
/* 淡入上滑（最通用，已内置在 html-template.md） */
.fragment.fade-up {
    opacity: 0; transform: translateY(20px);
    transition: opacity 0.5s, transform 0.5s cubic-bezier(0.16,1,0.3,1);
}
.fragment.fade-up.visible { opacity: 1; transform: translateY(0); }

/* 缩放进入 */
.fragment.scale-in {
    opacity: 0; transform: scale(0.85);
    transition: opacity 0.5s, transform 0.5s cubic-bezier(0.16,1,0.3,1);
}
.fragment.scale-in.visible { opacity: 1; transform: scale(1); }

/* 从左滑入 */
.fragment.slide-left {
    opacity: 0; transform: translateX(-40px);
    transition: opacity 0.5s, transform 0.5s cubic-bezier(0.16,1,0.3,1);
}
.fragment.slide-left.visible { opacity: 1; transform: translateX(0); }

/* 模糊进入 */
.fragment.blur-in {
    opacity: 0; filter: blur(8px);
    transition: opacity 0.6s, filter 0.6s cubic-bezier(0.16,1,0.3,1);
}
.fragment.blur-in.visible { opacity: 1; filter: blur(0); }
```

### 交错动画（同一页面内元素依次出现）

```css
.stagger > *:nth-child(1) { transition-delay: 0.1s; }
.stagger > *:nth-child(2) { transition-delay: 0.2s; }
.stagger > *:nth-child(3) { transition-delay: 0.3s; }
.stagger > *:nth-child(4) { transition-delay: 0.4s; }
.stagger > *:nth-child(5) { transition-delay: 0.5s; }
.stagger > *:nth-child(6) { transition-delay: 0.6s; }
```

---

## 内页背景效果

```css
/* 渐变网格 — 层叠径向渐变营造深度 */
.gradient-bg {
    background:
        radial-gradient(ellipse at 20% 80%, rgba(120,0,255,0.15) 0%, transparent 50%),
        radial-gradient(ellipse at 80% 20%, rgba(0,255,200,0.1) 0%, transparent 50%),
        var(--bg-primary);
}

/* 网格图案 — 科技/未来感 */
.grid-bg {
    background-image:
        linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
    background-size: 50px 50px;
}

/* 噪点纹理 — 增加质感 */
.noise-bg::after {
    content: '';
    position: absolute; inset: 0;
    opacity: 0.03;
    pointer-events: none;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
```

---

## ECharts 动画增强

```javascript
// 图表动态生长
const chartOption = {
    animationDuration: 1200,
    animationEasing: 'cubicOut',
    animationDelay: function (idx) { return idx * 150; }
};
```

---

## 故障排除

| 问题 | 原因 | 修复 |
|------|------|------|
| 内容页动画"一闪而过" | JS 动画和 Reveal.js slide 过渡同时播放 | 用 `setTimeout` 延迟 400ms，等 Reveal.js 过渡结束再启动 |
| 内容页动画"先出现再闪" | JS 先设 `opacity:0`（内容已可见），再动画显示 | 改用 CSS 预设 `opacity:0`，元素从一开始就隐藏，JS 只负责显示 |
| 封面动画不触发 | `.present` 在首屏加载时已存在，CSS transition 无状态变化 | 封面必须用 `@keyframes` 而非 transition |
| 离开再返回页面动画不重播 | 内联 style 残留 `opacity:1` | `resetSlideAnim` 清除内联样式，让 CSS 规则重新接管 `opacity:0` |
| 几何装饰不显示 | — | 检查父元素是否有 `position: relative` 和 `overflow: hidden` |
| `filter: blur` 影响性能 | — | 限制在封面页使用，内页用 `transform` + `opacity` |
| ECharts 动画闪烁 | — | 确认 `slidechanged` 后 300ms 再 `resize()` |

---

## 交互效果（可选增强）

### 3D Tilt 悬停效果 — 卡片/面板深度感

鼠标悬停在卡片上时产生 3D 倾斜效果，增强空间感。与 `.hl` 悬停高亮可共存。

```css
/* CSS 准备 — 加在卡片元素上 */
.tilt-card {
    transform-style: preserve-3d;
    perspective: 1000px;
    transition: transform 0.15s ease-out;
}
```

```javascript
/* JS 初始化 — 在 Reveal ready 后调用 */
function initTiltEffect() {
    document.querySelectorAll('.tilt-card').forEach(function(card) {
        card.addEventListener('mousemove', function(e) {
            var rect = card.getBoundingClientRect();
            var x = (e.clientX - rect.left) / rect.width - 0.5;
            var y = (e.clientY - rect.top) / rect.height - 0.5;
            card.style.transform = 'rotateY(' + (x * 8) + 'deg) rotateX(' + (-y * 8) + 'deg)';
        });
        card.addEventListener('mouseleave', function() {
            card.style.transform = 'rotateY(0) rotateX(0)';
        });
    });
}
```

**使用场景：** 品牌风格（Apple/Stripe/Linear）的卡片布局、产品特性展示、数据面板。不适合文字密集的列表页。

**注意：** `prefers-reduced-motion` 生效时应禁用此效果：
```javascript
if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;
```
| prefers-reduced-motion | — | 已在 viewport 规则中覆盖，动画降至 0.01ms |

---

### 数字计数器动画 — 数据页大数字滚动

页面出现时，`.data-big`、`.metric-val` 等数据数字从 0 滚动到目标值。适用于所有主题的数据概览页、指标展示页。

**HTML 标记 — 给目标数字加 `data-count` 属性：**
```html
<div class="data-big" data-count="71.7" data-suffix="%">0%</div>
<div class="data-big" data-count="9604" data-suffix="">0</div>
<div class="metric-val" data-count="61.7" data-suffix="%">0%</div>
```

**JS 实现 — 在 animateContentSlide 中调用：**
```javascript
function animateCounters(slide) {
    if (!slide) return;
    var counters = slide.querySelectorAll('[data-count]');
    counters.forEach(function(el) {
        var target = parseFloat(el.getAttribute('data-count'));
        var suffix = el.getAttribute('data-suffix') || '';
        var isFloat = String(target).indexOf('.') !== -1;
        var duration = 1200;
        var start = performance.now();
        function tick(now) {
            var progress = Math.min((now - start) / duration, 1);
            // easeOutCubic
            var ease = 1 - Math.pow(1 - progress, 3);
            var current = target * ease;
            el.textContent = (isFloat ? current.toFixed(1) : Math.round(current)) + suffix;
            if (progress < 1) requestAnimationFrame(tick);
        }
        requestAnimationFrame(tick);
    });
}
```

**触发时机：** 在 `animateContentSlide()` 末尾调用：
```javascript
// 在 animateContentSlide 函数末尾追加
animateCounters(Reveal.getCurrentSlide());
```

**resetSlideAnim 中重置：** 离开页面时将计数器归零：
```javascript
// 在 resetSlideAnim 函数中追加
p.querySelectorAll('[data-count]').forEach(function(el) {
    el.textContent = '0' + (el.getAttribute('data-suffix') || '');
});
```

**使用场景：** D-1/D-3/D-4 数据概览页、CH-1 图表旁的数据指标、S-2 总结页的数据条。纯文字页不需要。

---

### 粒子背景（Canvas）— Neon Cyber 签名元素

在背景层绘制漂浮粒子+连线，缓慢运动制造科技感"呼吸感"。仅建议 Neon Cyber 主题使用。

**HTML — 加在 `<body>` 开头：**
```html
<canvas id="particleCanvas" style="position:fixed;top:0;left:0;width:100%;height:100%;z-index:0;pointer-events:none;opacity:0.4"></canvas>
```

**JS 实现：**
```javascript
(function() {
    var canvas = document.getElementById('particleCanvas');
    if (!canvas) return;
    if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;
    var ctx = canvas.getContext('2d');
    var particles = [];
    var count = 40;
    var maxDist = 120;

    function resize() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
    resize();
    window.addEventListener('resize', resize);

    for (var i = 0; i < count; i++) {
        particles.push({
            x: Math.random() * canvas.width,
            y: Math.random() * canvas.height,
            vx: (Math.random() - 0.5) * 0.3,
            vy: (Math.random() - 0.5) * 0.3,
            r: Math.random() * 1.5 + 0.5
        });
    }

    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        for (var i = 0; i < particles.length; i++) {
            var p = particles[i];
            p.x += p.vx; p.y += p.vy;
            if (p.x < 0 || p.x > canvas.width) p.vx *= -1;
            if (p.y < 0 || p.y > canvas.height) p.vy *= -1;
            ctx.beginPath();
            ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
            ctx.fillStyle = 'rgba(0,255,204,0.6)';
            ctx.fill();
            // 连线
            for (var j = i + 1; j < particles.length; j++) {
                var q = particles[j];
                var dx = p.x - q.x, dy = p.y - q.y;
                var dist = Math.sqrt(dx * dx + dy * dy);
                if (dist < maxDist) {
                    ctx.beginPath();
                    ctx.moveTo(p.x, p.y);
                    ctx.lineTo(q.x, q.y);
                    ctx.strokeStyle = 'rgba(0,255,204,' + (0.15 * (1 - dist / maxDist)) + ')';
                    ctx.lineWidth = 0.5;
                    ctx.stroke();
                }
            }
        }
        requestAnimationFrame(draw);
    }
    draw();
})();
```

**使用场景：** 仅 Neon Cyber 主题。粒子颜色用主题的 `--accent`（青色），连线透明度随距离衰减。

**性能注意：**
- 粒子数控制在 40-60 个，超过 80 个低端设备会卡
- `opacity: 0.4` 确保粒子不抢内容视线
- `pointer-events: none` 不阻挡交互
- `prefers-reduced-motion` 时跳过初始化

---

## 叙事动画（Narrative Animation）— 基于 Reveal.js Fragment

> **核心理念：** 利用 Reveal.js 原生的 fragment 系统，给关键页面元素加 CSS 类实现"点击逐步出现"。不需要自定义 JS，不拦截翻页事件，零翻车风险。

### 为什么用 Fragment 而不是自定义动画？

| | 自定义动画（旧方案） | Fragment（当前方案） |
|---|---|---|
| JS 改动 | 重写 animateContentSlide + 事件拦截 | 零 JS 改动 |
| 翻页冲突 | 需要拦截 click/key/wheel 事件，容易翻车 | Reveal.js 内置处理，无冲突 |
| 后退支持 | 需要自己实现逆序隐藏 | Reveal.js 自动处理 |
| 代码量 | ~80行 JS | 8行 CSS |

### 使用方式

给需要逐步出现的 HTML 元素加 `class="fragment"` 或 `class="fragment impact"`：

```html
<!-- 焦点数字：点击弹入 -->
<div class="fragment impact" style="flex:1.2;background:...;padding:20px">
  <div style="font-size:3.2rem;font-weight:200;color:var(--accent)">71.7%</div>
  <div style="font-size:0.85rem">SWE-bench Verified 最高解决率</div>
</div>

<!-- 数据卡片：点击淡入上移 -->
<div class="card hl fragment">...</div>
<div class="card hl fragment">...</div>
<div class="card hl fragment">...</div>
```

### 交互行为（Reveal.js 内置）

- 翻到含 fragment 的页面 → fragment 元素隐藏
- 点击/空格/右箭头 → 显示下一个 fragment
- 所有 fragment 显示后 → 再次点击翻到下一页
- 左箭头/右键 → 逆序隐藏 fragment，全部隐藏后翻到上一页
- 导航栏点击 → 直接跳转，不受 fragment 影响

### CSS（在 boilerplate.html 的 `</style>` 前添加）

```css
/* Fragment 叙事动画 */
.reveal .slides section .fragment { opacity: 0; transform: translateY(15px); transition: opacity 0.6s cubic-bezier(0.25,0.46,0.45,0.94), transform 0.6s cubic-bezier(0.25,0.46,0.45,0.94); }
.reveal .slides section .fragment.visible { opacity: 1; transform: none; }
/* impact: 焦点数字弹入 */
.reveal .slides section .fragment.impact { opacity: 0; transform: scale(0.85) translateY(10px); }
.reveal .slides section .fragment.impact.visible { opacity: 1; transform: scale(1) translateY(0); transition: opacity 0.6s cubic-bezier(0.34,1.56,0.64,1), transform 0.6s cubic-bezier(0.34,1.56,0.64,1); }
```

### animateContentSlide 调整

只需跳过 fragment 元素，让 Reveal.js 管理：

```javascript
els.forEach(function(el, i) {
    if (el.classList.contains('fragment')) return; // 跳过，Reveal.js 管理
    // 默认 stagger 淡入
    el.style.transition = '...';
    el.style.opacity = '1';
    el.style.transform = 'translateY(0)';
});
```

### 使用频率指南

| PPT规模 | 需要 fragment 的页面 | 说明 |
|---------|---------------------|------|
| 5页 | 1-2页 | 核心数据页 + 结论页 |
| 10页 | 2-3页 | 焦点数据页 + 结论页 + 可选1页 |
| 15页 | 3-4页 | 2个焦点数据页 + 结论页 + 可选1页 |
| 20页 | 4-5页 | 3个焦点数据页 + 结论页 + 可选1-2页 |

**不是每页都要 fragment。** 大部分页面用默认 stagger 入场就够。只在"这个数字需要制造冲击"或"这条结论需要逐步揭示"时加 fragment。

### 无障碍降级

```css
@media (prefers-reduced-motion: reduce) {
    .reveal .slides section .fragment { opacity: 1 !important; transform: none !important; }
}
```
