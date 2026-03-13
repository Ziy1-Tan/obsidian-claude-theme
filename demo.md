# Claude-Like Theme Demo

> A calm, restrained reading experience for Obsidian — inspired by Claude Code's visual aesthetic.

---

## Typography

正文使用衬线字体，适合长文阅读。中英文混排时字距自然，段落间距宽松。**加粗文字**用于强调，*斜体*表示引用或术语，~~删除线~~表示废弃内容。

支持 ==高亮标记== 和 `行内代码`，以及 <kbd>Cmd</kbd> + <kbd>K</kbd> 这样的键盘快捷键。

外部链接：[Claude Code 官网](https://claude.ai/code)
内部链接：[[demo]]

---

## Headings

# H1 — 文档标题
## H2 — 章节标题
### H3 — 小节标题
#### H4 — 段落标题
##### H5 — 细分标题
###### H6 — 注释级标题

---

## Code

行内代码：`const greeting = "Hello, Claude"`

```python
# Python — syntax highlighting demo
import os
from typing import Optional

def greet(name: str, times: int = 1) -> Optional[str]:
    """Return a greeting string."""
    if not name:
        return None
    return "\n".join([f"Hello, {name}!"] * times)

class Config:
    DEBUG = False
    MAX_RETRIES = 3
    API_KEY = os.environ.get("API_KEY", "")

result = greet("Claude", times=2)
print(result)  # Hello, Claude!
```

```javascript
// JavaScript
const fetchData = async (url) => {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return { ok: true, data };
  } catch (error) {
    console.error("Request failed:", error.message);
    return { ok: false, error };
  }
};
```

```bash
# Shell
git clone git@github.com:user/repo.git
cd repo && npm install
npm run build -- --mode production
```

---

## Blockquote

> 真正的简洁不是删去所有装饰，而是让每一处细节都恰到好处。
>
> — 设计哲学

> 这是一段较长的引用块，用于展示多行引用时的排版效果。引用块有暖色背景和左侧边框，视觉上与正文区分，但不过分喧宾夺主。

---

## Callouts

> [!note] Note
> 这是一个普通提示，用于补充说明。

> [!tip] Tip
> 这是一个技巧提示，背景为绿色调。

> [!warning] Warning
> 这是一个警告，请注意潜在的风险。

> [!danger] Danger
> 这是一个危险提示，操作不可逆。

> [!quote] Quote
> 引用类 callout，风格与 blockquote 一致。

---

## Tables

| 属性 | 亮色模式 | 暗色模式 |
|------|---------|---------|
| 主背景 | `#faf9f5` 暖白 | `#151210` 极黑 |
| 正文颜色 | `#2b2621` 暖近黑 | `#ddd4ca` 暖米白 |
| Accent | `#bc6a3a` 烧橙 | `#d59567` 浅橙 |
| 代码背景 | `#f2eeea` | `#241d19` |
| 引用背景 | `#f3ede5` | `#1c1815` |

| 语言 | 关键字色 | 字符串色 | 函数色 |
|------|---------|---------|-------|
| 亮色 | `#7b2ff7` 紫 | `#2f8f2f` 绿 | `#2c5ec6` 蓝 |
| 暗色 | `#b899ff` 浅紫 | `#81cb82` 浅绿 | `#89b1ea` 浅蓝 |

---

## Lists

### Unordered

- 设计参考 Typora Claude-Like Theme
- 色调来自 Claude Code 官方网站
- 支持中英文混排，适合技术文档
  - 嵌套列表第二层
  - 保持视觉层级清晰
    - 第三层嵌套

### Ordered

1. 安装主题到 Obsidian
2. 在 Settings → Appearance 中选择 Claude-Like
3. 切换亮色或暗色模式
4. 享受阅读体验

### Task List

- [x] 颜色变量系统（亮色 + 暗色）
- [x] 排版与标题样式
- [x] 代码块与 Syntax Highlighting
- [x] 引用块与 Callout
- [x] 表格样式
- [x] 侧边栏与 Tag
- [ ] Style Settings 插件支持
- [ ] 发布到社区主题库

---

## Tags

#obsidian #theme #claude #design #markdown

---

## Horizontal Rule

上方内容结束。

---

下方内容开始。

---

## Images

![Placeholder](https://via.placeholder.com/800x400/faf9f5/bc6a3a?text=Claude-Like+Theme)

---

## Footnotes

这段话包含一个脚注[^1]，以及另一个脚注[^2]。

[^1]: 第一个脚注：主题设计灵感来自 Claude Code 官方网站的视觉风格。
[^2]: 第二个脚注：Typora 参考主题由 Muyiiiii 制作，见 GitHub 仓库。

---

## Math

行内公式：$E = mc^2$

块级公式：

$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$

---

*Claude-Like Theme · v0.1.0 · Made for Obsidian*
