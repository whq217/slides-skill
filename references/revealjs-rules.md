---
description: Reveal.js 关键技术要点 — 缩放机制、CSS 优先级、全屏背景架构、动画规则。生成 HTML 前必读，违反会导致功能失效。
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

