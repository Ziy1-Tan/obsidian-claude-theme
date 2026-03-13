# Obsidian Claude-Like Theme Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 创建一个 Obsidian 主题，还原 Claude Code 官网的视觉风格——暖米白底色、烧橙 accent、衬线正文字体、精细 syntax highlighting，支持亮色和暗色模式。

**Architecture:** 单文件 `theme.css`，通过 `.theme-light` / `.theme-dark` 类覆盖 Obsidian CSS 变量。设计 token 直接参考 Typora Claude-Like Theme（`#faf9f5` 暖白底、`#bc6a3a` 烧橙 accent），扩展覆盖 Obsidian 独有 UI（侧边栏、Callout、Tag、Graph）。

**Tech Stack:** Pure CSS，无外部资源，兼容 Obsidian 1.0+

---

## 文件结构

```
~/projects/obsidian-claude-theme/
├── manifest.json          # 主题元信息
├── theme.css              # 全部样式
└── docs/
    └── 2026-03-13-obsidian-claude-theme.md   # 本计划
```

`theme.css` 内部 section 组织：
1. `body` 共享变量（字体、圆角、间距）
2. `.theme-light` 颜色变量
3. `.theme-dark` 颜色变量
4. 排版基础（正文、段落、行高）
5. 标题（h1–h6）
6. 行内代码 + 代码块
7. Syntax Highlighting（CodeMirror 6）
8. 引用块（Blockquote）
9. Callout 块
10. 表格
11. 列表 + 任务
12. 侧边栏 / 文件树
13. Tag pills
14. 链接
15. 搜索高亮 / 选中态
16. Graph view 微调
17. 杂项（hr、kbd、图片等）

---

## Chunk 1: 基础脚手架 + 颜色变量

### Task 1: 创建 manifest.json

**Files:**
- Create: `manifest.json`

- [ ] **Step 1: 创建 manifest.json**

```json
{
  "name": "Claude-Like",
  "version": "0.1.0",
  "minAppVersion": "1.0.0",
  "author": "tanziyi",
  "authorUrl": ""
}
```

- [ ] **Step 2: 验证 JSON 合法**

```bash
python3 -m json.tool manifest.json
```
Expected: 输出格式化 JSON，无报错

- [ ] **Step 3: Commit**

```bash
git init
git add manifest.json
git commit -m "chore: init theme project with manifest"
```

---

### Task 2: theme.css — body 共享变量 + .theme-light 颜色系统

**Files:**
- Create: `theme.css`

设计 token 来源（Typora Claude-Like Theme 原始值）：

**亮色：**
- bg: `#faf9f5`，sidebar-bg: `#f5f2ec`
- text: `#2b2621`，muted: `#72695e`，faint: `#a8998a`
- heading: `#1c1815`
- accent: `#bc6a3a`，accent-hover: `#9d552d`
- border: `#ddd5ca`
- blockquote-bg: `#f3ede5`，blockquote-border: `#d8cbbb`，blockquote-text: `#625950`
- inline-code fg: `#b14a40`，bg: `#f2eeea`，border: `#d7cec5`
- code-block bg: `#fcfcfa`，border: `#e9e2d8`
- table-alt: `#f8f3ec`
- selection: `#e8ddd0`
- hover: `rgba(194,113,62,0.08)`
- active-file: `rgba(194,113,62,0.12)`

**暗色：**
- bg: `#151210`，sidebar-bg: `#171411`
- text: `#ddd4ca`，muted: `#9f8f81`，faint: `#6b5e53`
- heading: `#f2e9df`
- accent: `#d59567`，accent-hover: `#efb892`
- border: `#332b25`
- blockquote-bg: `#1c1815`，blockquote-border: `#4c4037`，blockquote-text: `#d0c2b4`
- inline-code fg: `#e1a092`，bg: `#241d19`，border: `#3a3029`
- code-block bg: `#1d1917`，border: `#2d2520`
- table-alt: `#1e1a17`
- selection: `rgba(213,149,103,0.25)`
- hover: `rgba(213,149,103,0.08)`
- active-file: `rgba(213,149,103,0.14)`

- [ ] **Step 1: 写 body 共享变量块**

```css
/* ─── Shared Variables ─────────────────────────────────────── */
body {
  /* Accent HSL — warm orange */
  --accent-h: 22;
  --accent-s: 54%;
  --accent-l: 47%;

  /* Typography */
  --font-text-theme: "Songti SC", "Noto Serif CJK SC", "Source Han Serif SC", Georgia, "Times New Roman", serif;
  --font-interface-theme: "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "Segoe UI", system-ui, sans-serif;
  --font-monospace-theme: "SF Mono", Menlo, Monaco, Consolas, "Cascadia Code", "Sarasa Mono SC", monospace;
  --font-text-size: 16px;
  --line-height: 1.65;

  /* Layout */
  --line-width: 42rem;
  --line-width-adaptive: var(--line-width);
  --max-width: 900px;

  /* Radius */
  --radius-s: 4px;
  --radius-m: 8px;
  --radius-l: 10px;
  --radius-xl: 999px;

  /* Heading sizes */
  --h1-size: 1.85em;
  --h2-size: 1.5em;
  --h3-size: 1.25em;
  --h4-size: 1.1em;
  --h5-size: 1em;
  --h6-size: 0.9em;

  /* Heading weight */
  --h1-weight: 600;
  --h2-weight: 600;
  --h3-weight: 600;
  --h4-weight: 600;
  --h5-weight: 600;
  --h6-weight: 600;

  /* Heading line height */
  --h1-line-height: 1.25;
  --h2-line-height: 1.3;
  --h3-line-height: 1.35;
}
```

- [ ] **Step 2: 写 .theme-light 颜色变量块**

```css
/* ─── Light Theme ───────────────────────────────────────────── */
.theme-light {
  color-scheme: light;

  /* Base backgrounds */
  --background-primary: #faf9f5;
  --background-primary-alt: #f6f3ed;
  --background-secondary: #f5f2ec;
  --background-secondary-alt: #ede8e0;
  --background-modifier-hover: rgba(194, 113, 62, 0.08);
  --background-modifier-active-hover: rgba(194, 113, 62, 0.12);
  --background-modifier-border: #ddd5ca;
  --background-modifier-border-hover: #cbb9a6;
  --background-modifier-border-focus: #bc6a3a;
  --background-modifier-form-field: #faf9f5;
  --background-modifier-form-field-highlighted: #ffffff;
  --background-modifier-box-shadow: rgba(43, 38, 33, 0.12);
  --background-modifier-cover: rgba(43, 38, 33, 0.6);
  --background-modifier-success: rgba(47, 143, 47, 0.1);
  --background-modifier-error: rgba(177, 74, 64, 0.1);
  --background-modifier-error-rgb: 177, 74, 64;
  --background-modifier-message: rgba(43, 38, 33, 0.06);

  /* Text */
  --text-normal: #2b2621;
  --text-muted: #72695e;
  --text-faint: #a8998a;
  --text-on-accent: #faf9f5;
  --text-on-accent-inverted: #bc6a3a;
  --text-error: #b14a40;
  --text-warning: #c06a2a;
  --text-success: #2f8f2f;
  --text-selection: #e8ddd0;
  --text-accent: #bc6a3a;
  --text-accent-hover: #9d552d;
  --text-highlight-bg: rgba(232, 221, 208, 0.7);
  --text-highlight-bg-active: rgba(232, 221, 208, 1);

  /* Headings */
  --h1-color: #1c1815;
  --h2-color: #1c1815;
  --h3-color: #2b2621;
  --h4-color: #2b2621;
  --h5-color: #3a312a;
  --h6-color: #625950;

  /* Interactive */
  --interactive-normal: #f3ede5;
  --interactive-hover: #ede3d8;
  --interactive-accent: #bc6a3a;
  --interactive-accent-hsl: 22, 54%, 47%;
  --interactive-accent-hover: #9d552d;

  /* Color palette */
  --color-accent: #bc6a3a;
  --color-accent-1: #c97a4e;
  --color-accent-2: #9d552d;
  --color-red: #b14a40;
  --color-green: #2f8f2f;
  --color-orange: #c06a2a;
  --color-blue: #2c5ec6;
  --color-purple: #7b2ff7;
  --color-cyan: #1a8a8a;
  --color-yellow: #a07820;
  --color-pink: #c03870;

  /* Base scale */
  --color-base-00: #ffffff;
  --color-base-10: #faf9f5;
  --color-base-20: #f5f2ec;
  --color-base-25: #ede8e0;
  --color-base-30: #ddd5ca;
  --color-base-35: #cbb9a6;
  --color-base-40: #b8a090;
  --color-base-50: #a8998a;
  --color-base-60: #72695e;
  --color-base-70: #625950;
  --color-base-100: #1c1815;

  /* Nav / sidebar */
  --nav-item-color: #72695e;
  --nav-item-color-hover: #2b2621;
  --nav-item-color-active: #bc6a3a;
  --nav-item-background-hover: rgba(194, 113, 62, 0.08);
  --nav-item-background-active: rgba(194, 113, 62, 0.12);
  --nav-collapse-icon-color: #a8998a;
  --nav-indentation-guide-color: rgba(194, 113, 62, 0.2);

  /* Tags */
  --tag-background: rgba(188, 106, 58, 0.1);
  --tag-color: #9d552d;
  --tag-color-hover: #bc6a3a;
  --tag-background-hover: rgba(188, 106, 58, 0.18);

  /* Checkbox / task */
  --checkbox-color: #bc6a3a;
  --checkbox-color-hover: #9d552d;
  --checkbox-border-color: #b8a090;
  --checkbox-border-color-hover: #bc6a3a;
  --checklist-done-color: #a8998a;
  --checklist-done-decoration: line-through;

  /* Link */
  --link-color: #bc6a3a;
  --link-color-hover: #9d552d;
  --link-unresolved-color: #a8998a;
  --link-external-color: #2c5ec6;
  --link-external-color-hover: #1e4a9f;
  --link-unresolved-opacity: 0.7;

  /* Blockquote */
  --blockquote-background-color: #f3ede5;
  --blockquote-border-color: #d8cbbb;
  --blockquote-color: #625950;

  /* Table */
  --table-background: transparent;
  --table-border-color: #ddd5ca;
  --table-border-width: 1px;
  --table-header-background: #f3ede5;
  --table-header-background-hover: #ede3d8;
  --table-column-alt-background: #f8f3ec;
  --table-row-background-hover: rgba(188, 106, 58, 0.05);
  --table-selection: rgba(188, 106, 58, 0.1);
  --table-header-color: #2b2621;

  /* Code inline */
  --code-normal: #b14a40;
  --code-background: #f2eeea;
  --code-comment: #6f665d;
  --code-function: #2c5ec6;
  --code-important: #7b2ff7;
  --code-keyword: #7b2ff7;
  --code-operator: #c06a2a;
  --code-property: #bc6a3a;
  --code-punctuation: #72695e;
  --code-string: #2f8f2f;
  --code-tag: #b14a40;
  --code-value: #c06a2a;

  /* Scrollbar */
  --scrollbar-thumb-bg: rgba(168, 153, 138, 0.4);
  --scrollbar-active-thumb-bg: rgba(168, 153, 138, 0.7);
}
```

- [ ] **Step 3: 写 .theme-dark 颜色变量块**

```css
/* ─── Dark Theme ────────────────────────────────────────────── */
.theme-dark {
  color-scheme: dark;

  /* Base backgrounds */
  --background-primary: #151210;
  --background-primary-alt: #1a1613;
  --background-secondary: #171411;
  --background-secondary-alt: #1d1915;
  --background-modifier-hover: rgba(213, 149, 103, 0.08);
  --background-modifier-active-hover: rgba(213, 149, 103, 0.14);
  --background-modifier-border: #332b25;
  --background-modifier-border-hover: #4c4037;
  --background-modifier-border-focus: #d59567;
  --background-modifier-form-field: #1d1915;
  --background-modifier-form-field-highlighted: #241d19;
  --background-modifier-box-shadow: rgba(0, 0, 0, 0.4);
  --background-modifier-cover: rgba(0, 0, 0, 0.7);
  --background-modifier-success: rgba(129, 203, 130, 0.12);
  --background-modifier-error: rgba(225, 160, 146, 0.12);
  --background-modifier-error-rgb: 225, 160, 146;
  --background-modifier-message: rgba(255, 255, 255, 0.05);

  /* Text */
  --text-normal: #ddd4ca;
  --text-muted: #9f8f81;
  --text-faint: #6b5e53;
  --text-on-accent: #151210;
  --text-on-accent-inverted: #d59567;
  --text-error: #e1a092;
  --text-warning: #d9a56f;
  --text-success: #81cb82;
  --text-selection: rgba(213, 149, 103, 0.25);
  --text-accent: #d59567;
  --text-accent-hover: #efb892;
  --text-highlight-bg: rgba(213, 149, 103, 0.2);
  --text-highlight-bg-active: rgba(213, 149, 103, 0.3);

  /* Headings */
  --h1-color: #f2e9df;
  --h2-color: #f2e9df;
  --h3-color: #e8ddd0;
  --h4-color: #ddd4ca;
  --h5-color: #cdc0b2;
  --h6-color: #9f8f81;

  /* Interactive */
  --interactive-normal: #241d19;
  --interactive-hover: #2d2520;
  --interactive-accent: #d59567;
  --interactive-accent-hsl: 29, 57%, 58%;
  --interactive-accent-hover: #efb892;

  /* Color palette */
  --color-accent: #d59567;
  --color-accent-1: #efb892;
  --color-accent-2: #c07848;
  --color-red: #e1a092;
  --color-green: #81cb82;
  --color-orange: #d9a56f;
  --color-blue: #89b1ea;
  --color-purple: #b899ff;
  --color-cyan: #5ec4c4;
  --color-yellow: #d4b56a;
  --color-pink: #e080a8;

  /* Base scale */
  --color-base-00: #0e0c0a;
  --color-base-10: #151210;
  --color-base-20: #171411;
  --color-base-25: #1d1915;
  --color-base-30: #332b25;
  --color-base-35: #4c4037;
  --color-base-40: #6b5e53;
  --color-base-50: #7d6e62;
  --color-base-60: #9f8f81;
  --color-base-70: #b3a392;
  --color-base-100: #f2e9df;

  /* Nav / sidebar */
  --nav-item-color: #9f8f81;
  --nav-item-color-hover: #ddd4ca;
  --nav-item-color-active: #d59567;
  --nav-item-background-hover: rgba(213, 149, 103, 0.08);
  --nav-item-background-active: rgba(213, 149, 103, 0.14);
  --nav-collapse-icon-color: #6b5e53;
  --nav-indentation-guide-color: rgba(213, 149, 103, 0.18);

  /* Tags */
  --tag-background: rgba(213, 149, 103, 0.12);
  --tag-color: #d59567;
  --tag-color-hover: #efb892;
  --tag-background-hover: rgba(213, 149, 103, 0.22);

  /* Checkbox / task */
  --checkbox-color: #d59567;
  --checkbox-color-hover: #efb892;
  --checkbox-border-color: #6b5e53;
  --checkbox-border-color-hover: #d59567;
  --checklist-done-color: #6b5e53;
  --checklist-done-decoration: line-through;

  /* Link */
  --link-color: #d59567;
  --link-color-hover: #efb892;
  --link-unresolved-color: #6b5e53;
  --link-external-color: #89b1ea;
  --link-external-color-hover: #aac8f0;
  --link-unresolved-opacity: 0.6;

  /* Blockquote */
  --blockquote-background-color: #1c1815;
  --blockquote-border-color: #4c4037;
  --blockquote-color: #d0c2b4;

  /* Table */
  --table-background: transparent;
  --table-border-color: #332b25;
  --table-border-width: 1px;
  --table-header-background: #1d1915;
  --table-header-background-hover: #241d19;
  --table-column-alt-background: #1e1a17;
  --table-row-background-hover: rgba(213, 149, 103, 0.05);
  --table-selection: rgba(213, 149, 103, 0.12);
  --table-header-color: #ddd4ca;

  /* Code inline */
  --code-normal: #e1a092;
  --code-background: #241d19;
  --code-comment: #9f8f81;
  --code-function: #89b1ea;
  --code-important: #b899ff;
  --code-keyword: #b899ff;
  --code-operator: #d9a56f;
  --code-property: #d59567;
  --code-punctuation: #9f8f81;
  --code-string: #81cb82;
  --code-tag: #e1a092;
  --code-value: #d9a56f;

  /* Scrollbar */
  --scrollbar-thumb-bg: rgba(107, 94, 83, 0.5);
  --scrollbar-active-thumb-bg: rgba(107, 94, 83, 0.8);
}
```

- [ ] **Step 4: 保存文件，在 Obsidian 中安装测试**

将 `theme.css` 和 `manifest.json` 复制到 Obsidian vault 的 `.obsidian/themes/Claude-Like/` 目录，在 Settings → Appearance 中选择 Claude-Like 主题，切换亮色/暗色验证底色和文字颜色正确。

- [ ] **Step 5: Commit**

```bash
git add theme.css
git commit -m "feat: add color variable system for light and dark themes"
```

---

## Chunk 2: 排版、标题、正文

### Task 3: 排版基础 + 标题样式

**Files:**
- Modify: `theme.css` (append)

- [ ] **Step 1: 追加排版基础样式**

```css
/* ─── Typography Base ───────────────────────────────────────── */
body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.markdown-preview-view,
.markdown-source-view.mod-cm6 .cm-content {
  font-family: var(--font-text-theme);
  line-height: var(--line-height);
  font-size: var(--font-text-size);
}

/* Reading view paragraph spacing */
.markdown-preview-view p {
  margin-block-start: 0;
  margin-block-end: 0.9em;
}

/* Horizontal rule */
.markdown-preview-view hr {
  border: none;
  border-top: 1px solid var(--background-modifier-border);
  opacity: 0.75;
  margin: 2em 0;
}
```

- [ ] **Step 2: 追加标题样式**

```css
/* ─── Headings ──────────────────────────────────────────────── */
.markdown-preview-view h1,
.markdown-preview-view h2,
.markdown-preview-view h3,
.markdown-preview-view h4,
.markdown-preview-view h5,
.markdown-preview-view h6 {
  font-family: var(--font-interface-theme);
  font-weight: 600;
  line-height: 1.3;
  margin-top: 1.5em;
  margin-bottom: 0.5em;
}

/* h2 bottom border — Typora 风格 */
.markdown-preview-view h2 {
  border-bottom: 1px solid var(--background-modifier-border);
  padding-bottom: 0.3em;
}

/* Editor mode headings */
.cm-line.HyperMD-header {
  font-family: var(--font-interface-theme);
  font-weight: 600;
}
```

- [ ] **Step 3: 验证标题在 Reading + Source 两种视图下均正确渲染**

打开一篇包含 h1–h6 的笔记，切换 Reading/Source 视图确认字体、颜色、间距符合预期。

- [ ] **Step 4: Commit**

```bash
git add theme.css
git commit -m "feat: add typography base and heading styles"
```

---

## Chunk 3: 代码样式

### Task 4: 行内代码 + 代码块 + Syntax Highlighting

**Files:**
- Modify: `theme.css` (append)

- [ ] **Step 1: 追加行内代码样式**

```css
/* ─── Inline Code ───────────────────────────────────────────── */
.markdown-preview-view code:not(pre code),
.cm-inline-code {
  font-family: var(--font-monospace-theme);
  font-size: 0.875em;
  color: var(--code-normal);
  background-color: var(--code-background);
  border: 1px solid var(--background-modifier-border);
  border-radius: var(--radius-xl);
  padding: 0.15em 0.45em;
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.04);
}

.theme-dark .markdown-preview-view code:not(pre code),
.theme-dark .cm-inline-code {
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.2);
}
```

- [ ] **Step 2: 追加代码块样式**

```css
/* ─── Code Blocks ───────────────────────────────────────────── */
.markdown-preview-view pre {
  font-family: var(--font-monospace-theme);
  font-size: 0.875em;
  line-height: 1.6;
  background-color: var(--code-background);
  border: 1px solid var(--background-modifier-border);
  border-radius: var(--radius-m);
  padding: 1em 1.2em;
  overflow-x: auto;
  box-shadow: 0 1px 3px var(--background-modifier-box-shadow);
}

.markdown-preview-view pre code {
  background: none;
  border: none;
  border-radius: 0;
  padding: 0;
  font-size: inherit;
  color: var(--text-normal);
  box-shadow: none;
}

/* CM6 editor code block */
.HyperMD-codeblock {
  font-family: var(--font-monospace-theme) !important;
  font-size: 0.875em;
  line-height: 1.6;
}

.HyperMD-codeblock-begin,
.HyperMD-codeblock-end {
  border-radius: var(--radius-m);
}

/* Code block background line highlight */
.cm-line.HyperMD-codeblock {
  background-color: var(--code-background);
  padding-inline: 0.8em;
}
```

- [ ] **Step 3: 追加 Syntax Highlighting (CodeMirror 6 token classes)**

```css
/* ─── Syntax Highlighting ───────────────────────────────────── */
/* Keywords: if, for, return, import, etc. */
.cm-keyword { color: var(--code-keyword); }
/* Strings */
.cm-string, .cm-string-2 { color: var(--code-string); }
/* Numbers, booleans */
.cm-number, .cm-atom { color: var(--code-value); }
/* Function names */
.cm-def, .cm-variable-2 { color: var(--code-function); }
/* Class names / types */
.cm-type, .cm-variable-3 { color: var(--code-function); }
/* Operators */
.cm-operator { color: var(--code-operator); }
/* Comments */
.cm-comment { color: var(--code-comment); font-style: italic; }
/* Properties */
.cm-property, .cm-attribute { color: var(--code-property); }
/* Tags (HTML/JSX) */
.cm-tag { color: var(--code-tag); }
/* Punctuation */
.cm-punctuation, .cm-bracket, .cm-meta { color: var(--code-punctuation); }
/* Built-in functions */
.cm-builtin { color: var(--code-important); }

/* Prism.js classes (Reading mode) */
.token.keyword { color: var(--code-keyword); }
.token.string, .token.char { color: var(--code-string); }
.token.number, .token.boolean { color: var(--code-value); }
.token.function { color: var(--code-function); }
.token.class-name { color: var(--code-function); }
.token.operator { color: var(--code-operator); }
.token.comment { color: var(--code-comment); font-style: italic; }
.token.property, .token.attr-name { color: var(--code-property); }
.token.tag { color: var(--code-tag); }
.token.punctuation { color: var(--code-punctuation); }
.token.builtin, .token.important { color: var(--code-important); }
```

- [ ] **Step 4: 验证代码样式**

打开包含行内代码、代码块（Python/JS/Shell）的笔记，确认：
- 行内代码有胶囊圆角、暖色底、深橙文字
- 代码块有轻微阴影和圆角
- syntax highlighting 颜色正确（紫关键字、绿字符串、蓝函数）

- [ ] **Step 5: Commit**

```bash
git add theme.css
git commit -m "feat: add inline code, code block, and syntax highlighting styles"
```

---

## Chunk 4: 引用块、Callout、表格

### Task 5: Blockquote + Callout

**Files:**
- Modify: `theme.css` (append)

- [ ] **Step 1: 追加 Blockquote 样式**

```css
/* ─── Blockquote ────────────────────────────────────────────── */
.markdown-preview-view blockquote {
  background-color: var(--blockquote-background-color);
  border-left: 2px solid var(--blockquote-border-color);
  border-radius: var(--radius-l);
  color: var(--blockquote-color);
  margin: 1em 0;
  padding: 0.7em 1.2em;
  font-style: normal;
}

.markdown-preview-view blockquote p {
  margin-bottom: 0;
}
```

- [ ] **Step 2: 追加 Callout 基础样式（5 种类型）**

```css
/* ─── Callout ───────────────────────────────────────────────── */
.callout {
  border-radius: var(--radius-l);
  padding: 0.8em 1.2em;
  border-left-width: 3px;
  border-left-style: solid;
}

/* note / info */
.callout[data-callout="note"],
.callout[data-callout="info"] {
  --callout-color: 44, 94, 198;  /* blue */
  background-color: rgba(44, 94, 198, 0.07);
  border-left-color: #2c5ec6;
}

/* tip / success */
.callout[data-callout="tip"],
.callout[data-callout="success"],
.callout[data-callout="check"] {
  --callout-color: 47, 143, 47;  /* green */
  background-color: rgba(47, 143, 47, 0.07);
  border-left-color: #2f8f2f;
}

/* warning */
.callout[data-callout="warning"],
.callout[data-callout="caution"],
.callout[data-callout="attention"] {
  --callout-color: 192, 106, 42;  /* orange */
  background-color: rgba(192, 106, 42, 0.07);
  border-left-color: #c06a2a;
}

/* danger / error */
.callout[data-callout="danger"],
.callout[data-callout="error"],
.callout[data-callout="bug"] {
  --callout-color: 177, 74, 64;  /* red */
  background-color: rgba(177, 74, 64, 0.07);
  border-left-color: #b14a40;
}

/* quote / abstract */
.callout[data-callout="quote"],
.callout[data-callout="abstract"],
.callout[data-callout="summary"],
.callout[data-callout="tldr"] {
  --callout-color: 98, 89, 80;  /* warm gray */
  background-color: var(--blockquote-background-color);
  border-left-color: var(--blockquote-border-color);
}

/* Dark mode adjustments — increase opacity slightly */
.theme-dark .callout[data-callout="note"],
.theme-dark .callout[data-callout="info"] {
  background-color: rgba(137, 177, 234, 0.1);
}

.theme-dark .callout[data-callout="tip"],
.theme-dark .callout[data-callout="success"],
.theme-dark .callout[data-callout="check"] {
  background-color: rgba(129, 203, 130, 0.1);
}

.theme-dark .callout[data-callout="warning"],
.theme-dark .callout[data-callout="caution"],
.theme-dark .callout[data-callout="attention"] {
  background-color: rgba(217, 165, 111, 0.1);
  border-left-color: #d9a56f;
}

.theme-dark .callout[data-callout="danger"],
.theme-dark .callout[data-callout="error"],
.theme-dark .callout[data-callout="bug"] {
  background-color: rgba(225, 160, 146, 0.1);
  border-left-color: #e1a092;
}
```

- [ ] **Step 3: Commit**

```bash
git add theme.css
git commit -m "feat: add blockquote and callout styles"
```

### Task 6: 表格样式

**Files:**
- Modify: `theme.css` (append)

- [ ] **Step 1: 追加表格样式**

```css
/* ─── Table ─────────────────────────────────────────────────── */
.markdown-preview-view table {
  border-collapse: collapse;
  width: 100%;
  font-size: 0.92em;
  margin: 1em 0 1.5em;
}

.markdown-preview-view th {
  background-color: var(--table-header-background);
  color: var(--table-header-color);
  font-family: var(--font-interface-theme);
  font-weight: 600;
  padding: 0.55em 0.9em;
  text-align: left;
  border-bottom: 2px solid var(--background-modifier-border-hover);
  border-top: 1px solid var(--table-border-color);
}

.markdown-preview-view td {
  padding: 0.5em 0.9em;
  border-bottom: 1px solid var(--table-border-color);
  vertical-align: top;
}

/* Alternating rows */
.markdown-preview-view tr:nth-child(even) td {
  background-color: var(--table-column-alt-background);
}

.markdown-preview-view tr:hover td {
  background-color: var(--table-row-background-hover);
}
```

- [ ] **Step 2: Commit**

```bash
git add theme.css
git commit -m "feat: add table styles"
```

---

## Chunk 5: 侧边栏、Tag、链接、杂项

### Task 7: 侧边栏 / 文件树

**Files:**
- Modify: `theme.css` (append)

- [ ] **Step 1: 追加侧边栏样式**

```css
/* ─── Sidebar / File Explorer ───────────────────────────────── */
.nav-folder-title,
.nav-file-title {
  color: var(--nav-item-color);
  border-radius: var(--radius-s);
  padding: 2px 6px;
}

.nav-folder-title:hover,
.nav-file-title:hover {
  color: var(--nav-item-color-hover);
  background-color: var(--nav-item-background-hover);
}

.nav-file-title.is-active {
  color: var(--nav-item-color-active);
  background-color: var(--nav-item-background-active);
  font-weight: 500;
}

/* Collapse indicator */
.nav-folder-collapse-indicator {
  color: var(--nav-collapse-icon-color);
}

/* Indentation guide */
.nav-indentation-guide {
  border-left-color: var(--nav-indentation-guide-color);
}
```

- [ ] **Step 2: 追加 Tag pills 样式**

```css
/* ─── Tags ──────────────────────────────────────────────────── */
.tag {
  background-color: var(--tag-background);
  color: var(--tag-color);
  border: none;
  border-radius: var(--radius-xl);
  padding: 0.1em 0.6em;
  font-size: 0.82em;
  font-family: var(--font-interface-theme);
  text-decoration: none;
}

.tag:hover {
  background-color: var(--tag-background-hover);
  color: var(--tag-color-hover);
}
```

- [ ] **Step 3: 追加链接样式**

```css
/* ─── Links ─────────────────────────────────────────────────── */
.markdown-preview-view a.internal-link {
  color: var(--link-color);
  text-decoration: none;
  border-bottom: 1px solid rgba(188, 106, 58, 0.35);
  transition: border-color 0.15s, color 0.15s;
}

.markdown-preview-view a.internal-link:hover {
  color: var(--link-color-hover);
  border-bottom-color: var(--link-color-hover);
}

.markdown-preview-view a.external-link {
  color: var(--link-external-color);
  text-decoration: none;
  border-bottom: 1px solid rgba(44, 94, 198, 0.3);
}

.markdown-preview-view a.external-link:hover {
  color: var(--link-external-color-hover);
}

/* Unresolved links */
.markdown-preview-view a.is-unresolved {
  color: var(--link-unresolved-color);
  opacity: var(--link-unresolved-opacity);
  border-bottom-style: dashed;
}
```

- [ ] **Step 4: 追加杂项样式**

```css
/* ─── Miscellaneous ─────────────────────────────────────────── */

/* Scrollbar (WebKit) */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

::-webkit-scrollbar-thumb {
  background-color: var(--scrollbar-thumb-bg);
  border-radius: 999px;
}

::-webkit-scrollbar-thumb:hover {
  background-color: var(--scrollbar-active-thumb-bg);
}

::-webkit-scrollbar-track {
  background: transparent;
}

/* Keyboard shortcut / kbd */
.markdown-preview-view kbd {
  font-family: var(--font-monospace-theme);
  font-size: 0.8em;
  background-color: var(--background-secondary);
  border: 1px solid var(--background-modifier-border);
  border-bottom-width: 2px;
  border-radius: var(--radius-s);
  padding: 0.1em 0.4em;
  color: var(--text-muted);
}

/* Image */
.markdown-preview-view img {
  border-radius: var(--radius-m);
  max-width: 100%;
}

/* Bold / Strong */
.markdown-preview-view strong {
  color: var(--text-normal);
  font-weight: 700;
}

/* Italic */
.markdown-preview-view em {
  color: var(--text-muted);
}

/* Mark / Highlight */
.markdown-preview-view mark {
  background-color: var(--text-highlight-bg);
  color: var(--text-normal);
  border-radius: 2px;
  padding: 0 2px;
}

/* Lists */
.markdown-preview-view ul,
.markdown-preview-view ol {
  padding-left: 1.5em;
}

.markdown-preview-view li {
  margin-bottom: 0.2em;
}

/* Task list */
.task-list-item-checkbox {
  accent-color: var(--checkbox-color);
}

.task-list-item.is-checked .task-list-item-text {
  color: var(--checklist-done-color);
  text-decoration: var(--checklist-done-decoration);
}

/* Graph view node color */
.graph-view.color-fill {
  color: var(--interactive-accent);
}

.graph-view.color-line {
  color: var(--background-modifier-border);
}

.graph-view.color-text {
  color: var(--text-muted);
}

/* Frontmatter / properties panel */
.metadata-container {
  background-color: var(--background-secondary);
  border: 1px solid var(--background-modifier-border);
  border-radius: var(--radius-m);
  padding: 0.5em 1em;
  font-size: 0.88em;
  color: var(--text-muted);
}

/* Active line highlight (subtle) */
.cm-active-line {
  background-color: rgba(var(--accent-h), var(--accent-s), var(--accent-l), 0.03) !important;
}
```

- [ ] **Step 5: 验证整体效果**

打开一篇包含：标题、正文、代码、引用、表格、任务、tag、链接的笔记，亮暗模式各验证一遍，确认整体视觉一致、无元素缺漏。

- [ ] **Step 6: Commit**

```bash
git add theme.css
git commit -m "feat: add sidebar, tags, links, and miscellaneous styles"
```

---

## Chunk 6: 最终打包

### Task 8: 整理、README、安装说明

**Files:**
- Create: `README.md`

- [ ] **Step 1: 写 README.md**

内容包含：主题截图占位、安装步骤、字体推荐（Noto Serif CJK SC、Cascadia Code）、设计来源说明（参考 Typora Claude-Like Theme）。

- [ ] **Step 2: 验证 manifest.json 完整性**

确认 `name`、`version`、`minAppVersion` 三个必填字段均存在。

- [ ] **Step 3: Final commit**

```bash
git add .
git commit -m "chore: add README and finalize theme v0.1.0"
git tag v0.1.0
```

---

## 安装测试方法

```bash
# 创建 Obsidian 主题目录（替换为你的 vault 路径）
VAULT="$HOME/Documents/ob_notes"
THEME_DIR="$VAULT/.obsidian/themes/Claude-Like"
mkdir -p "$THEME_DIR"

# 复制主题文件
cp theme.css manifest.json "$THEME_DIR/"
```

然后在 Obsidian → Settings → Appearance → Themes 中选择 **Claude-Like**。
