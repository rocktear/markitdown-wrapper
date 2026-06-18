# doc2md — Microsoft MarkItDown 的万能壳

> 将 PDF、Word、PPT、Excel、图片、音频、YouTube、Wikipedia、RSS、ZIP...
> 一切能读的东西转为 Markdown。
> 一把刀，一个命令，零绑定。

## 为什么需要？

[Microsoft MarkItDown](https://github.com/microsoft/markitdown) 已经是文档→Markdown 的标准库了。
但它只有 Python API 和简单 CLI，没有封装以下三件事：

1. **自动判断格式** — 文件、URL、图片、音频、视频全混在一起时，该走哪个引擎？
2. **统一输出位置** — 每次转完散落在不同目录，谁记得住？
3. **LLM 图片描述** — markitdown 明明支持，但是没人把这条能力串起来。

`doc2md` 干的就是这三件事。包在一条命令里。

## 安装

```bash
# 1. 装 markitdown
pip install 'markitdown[all]'

# 2. 拿脚本
curl -Lo ~/.local/bin/doc2md https://raw.githubusercontent.com/rocktear/markitdown-wrapper/main/doc2md
chmod +x ~/.local/bin/doc2md

# 3. 验证
doc2md --help
```

依赖：Python ≥ 3.9, markitdown[all]。不绑任何 Agent 框架、不绑任何 API key——装上就能用。

## 使用

```bash
# 转文档
doc2md paper.pdf
doc2md report.docx slides.pptx data.xlsx

# 图片（EXIF 元数据）
doc2md photo.jpg

# 图片 + LLM 视觉描述
OPENAI_API_KEY=*** OPENAI_BASE_URL="http://localhost:PORT/v1" \
  DOC2MD_LLM_MODEL="gpt-4o" \
  doc2md screenshot.png

# 音频 → 文字
doc2md meeting.mp3

# 网络
doc2md "https://youtu.be/video-id"
doc2md "https://en.wikipedia.org/wiki/AI"
doc2md feed.xml   # RSS/Atom

# 批量
doc2md *.pdf ~/Downloads/*.docx
```

## 输出

默认输出到 `~/Documents/wiki/playbook/raw/`，可改：

```bash
# 自定义输出目录
export DOC2MD_OUTPUT=~/my-notes
doc2md paper.pdf   # → ~/my-notes/paper.md
```

## 支持格式

| 格式 | 说明 |
|------|------|
| PDF | 论文/报告/书籍，含表格提取 |
| DOCX | Word 文档 |
| PPTX | 课件/演示文稿 |
| XLSX/XLS | Excel 表格，多 sheet |
| HTML | 网页 |
| CSV/JSON/XML | 结构化数据 |
| IPYNB | Jupyter Notebook |
| MSG | Outlook 邮件 |
| EPUB | 电子书 |
| JPG/PNG/GIF | 图片（EXIF + 可选 LLM 描述） |
| MP3/WAV | 音频转录（Google Speech） |
| YouTube | 视频字幕 |
| Wikipedia | 维基文章 |
| RSS/Atom | 订阅源 |
| ZIP | 递归解压处理 |

## LLM 图片描述

```bash
# 必填
export OPENAI_API_KEY=***  可选
export OPENAI_BASE_URL="https://api.openai.com/v1"   # 默认
export DOC2MD_LLM_MODEL="gpt-4o"                      # 默认，需视觉模型
export DOC2MD_IMAGE_PROMPT="自定义描述提示词"         # 可选
```

不设置 → 只提取 EXIF 元数据。设置 → 额外调用 LLM 做视觉描述。

## 设计原则

**解耦，不绑定。**
- 不绑 API key
- 不绑目录结构（输出目录可配）
- 不绑 OpenAI（任何兼容接口都行）
- 不绑 Agent 框架
- 不绑任何特定平台

**一个文件，全局可用。**
- `doc2md` 就是它。
- 软链到 `~/.local/bin` 或放 `$PATH` 任何地方。

## License

MIT — 基于 Microsoft MarkItDown (MIT)