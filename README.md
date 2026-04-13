# Slides Skill — AI 驱动的交互式网页 PPT 生成器

> 基于 Reveal.js 的 Claude Code 技能包，从零生成专业级 HTML 演示文稿

## 这个技能做什么

**Slides Skill** 帮助用户通过 Claude Code 快速生成交互式网页演示文稿（PPT），无需 CSS/JS 知识。采用"先看效果再选"的工作流：自动生成风格预览，你挑选喜欢的风格后完成生成。

## 核心特性

- **零依赖** — 单个 HTML 文件，内联 CSS/JS，无需 npm 或构建工具
- **19 套主题预设** — 暗色/亮色/特色三类，每套有独特的字体、配色和签名元素
- **33 种布局模式** — 封面/数据/图表/列表/对比/图文卡片/特殊功能等，根据内容自动匹配
- **视觉风格发现** — 生成 3 种风格预览供你选择，不用文字描述审美偏好
- **页面入场动画** — CSS 预隐藏 + JS 渐显双轨方案，流畅无闪烁
- **ECharts 图表动画** — 按图表类型匹配不同入场节奏（折线/饼图/柱图/雷达图）
- **浏览器内编辑** — 按 E 键进入编辑模式，直接修改文字内容
- **画笔标注** — 按 D 键切换画笔，右键清除，适合演讲演示
- **演讲者模式** — 按 S 键打开独立演讲者窗口，显示当前页备注讲稿，支持实时编辑
- **PDF 导出** — 附带导出脚本，支持打印质量输出

## 快速开始

### 安装

将技能文件复制到 Claude Code skills 目录：

```bash
mkdir -p ~/.claude/skills/slides

cp SKILL.md STYLE_PRESETS.md LAYOUT_PATTERNS.md animation-patterns.md html-template.md viewport-base.css ~/.claude/skills/slides/
cp -r scripts ~/.claude/skills/slides/
```

### 使用

在 Claude Code 中说：

```
帮我做一个关于XXX的PPT
```

技能自动触发 5 阶段工作流：配置 → 调研 → 大纲 → 风格与封面 → 生成

### 操作方式

| 操作 | 方法 |
|------|------|
| 下一页 | 方向键 / 点击空白处 / 滚轮 |
| 上一页 | 右键点击空白处 |
| 编辑模式 | 按 E 键 |
| 画笔模式 | 按 D 键 |
| 演讲者模式 | 按 S 键 |
| 清除画笔 | 右键 |

## 技术栈

| 技术 | 用途 |
|------|------|
| Reveal.js 4.x | 演示框架 |
| ECharts 5.x | 图表渲染 |
| Font Awesome 6 | 图标库 |
| Google Fonts | 字体 |

## 项目结构

```
slides/
├── SKILL.md              # 主技能文件（5阶段工作流）
├── STYLE_PRESETS.md      # 19 套主题预设（字体/配色/签名元素）
├── LAYOUT_PATTERNS.md    # 33 种布局模式库
├── animation-patterns.md # 动画系统参考
├── html-template.md      # HTML 模板架构
├── viewport-base.css     # 响应式基础样式
└── scripts/
    ├── deploy.sh         # 部署脚本
    └── export-pdf.sh     # PDF 导出脚本
```

## 致谢

本项目基于 @zarazhangrui 的 [Frontend Slides](https://github.com/zarazhangrui/frontend-slides)（MIT 协议）构建。

由 Hongqing W 增强：33种布局模式库、页面入场动画系统、图表动画预设、演讲者模式、CDN 可访问性修复、Font Awesome 图标渲染修复等。

## 许可证

MIT License — 详见 [LICENSE](LICENSE)
