# Obsidian Claude Theme

An Obsidian theme inspired by [Claude](https://claude.com/product/overview), with light and dark modes.

## Reading and typography

Keep notes close to the work. Capture a question, add an example, and return to the conclusion when the evidence is clearer. Follow an [[#Code|internal link]] or visit [Claude](https://claude.com/product/overview).

午后整理读书笔记，把零散的想法写成几段文字。中文正文采用宋体，英文与数字穿插其中，可以一起观察行高、字重和标点的效果。

**Bold 重点文字**, *italic 斜体*, ~~strikethrough 删除线~~, ==highlight 高亮==, `inline code`, #notes and #读书笔记. Press <kbd>Cmd+K</kbd> to insert a link.

## Code

```python
def greet(name: str) -> str:
    """Return a greeting. 返回一条问候。"""
    return f"Hello, {name}!"  # 中文注释与英文标识符

class Config:
    DEBUG = False
    RETRIES = 3
```

## Blockquote and callouts

> Keep the question next to the evidence. 阅读时记下疑问，写作时补上出处。

> [!note] Note · 笔记
> Record the source and page number. 记录出处，方便下次查阅。

> [!tip] Tip · 提示
> Use [[#Reading and typography|internal links]] to connect related notes.

> [!warning] Warning · 注意
> Keep a copy before changing an important note. 修改前保留副本。

## Table and tasks

| Token | Light · 浅色 | Dark · 深色 |
|---|---|---|
| Keyword | `#5d55a8` purple | `#a891f5` |
| String | `#357059` green | `#8ed08f` |
| Function | `#33689c` blue | `#93baee` |

- [x] Color system, light and dark
- [x] Typography, code blocks and callouts
- [ ] Continue reading · 继续阅读下一章

---

*Obsidian Claude Theme · v0.2.0*
