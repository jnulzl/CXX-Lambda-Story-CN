# C++ Lambda 表达式全解（中文翻译版 · LaTeX 工程）

把 Bartłomiej Filipek 的 *C++ Lambda Story*（2022, Leanpub）全书译为中文，
并使用 Manning 风格 LaTeX 模板排版。本目录即编译工程根目录；原书英文 PDF 在上一层目录。

- 原书：*C++ Lambda Story — Everything you need to know about Lambda Expressions in Modern C++*
- 作者：Bartłomiej Filipek（cppstories.com）
- 中文名：**C++ Lambda 表达式全解**

---

## 目录结构

```
latex_source/                  # 本目录即工程根
├── main.tex                   # 编译入口：\input{book/ccs.tex} + \subfile{book/index.tex}
├── build.bat                  # ⭐ 本项目编译脚本：双击或 cmd /c build.bat
├── fonts/                     # 字体目录（代码等宽用系统自带 Consolas，无需额外字体文件）
├── images/                    # 原书封面 + 5 张正文插图 + icon_info/icon_warning（提示框图标）
├── main.pdf                   # 编译成品（约 125 页）
└── book/
    ├── ccs.tex                # 导言区：自定义命令/环境、minted v3 缓存配置、字体映射
    ├── index.tex              # 章节装配（手写静态文件，增删章节改这里）
    ├── manning-pygments-style.tex  # 代码高亮配色
    └── content/
        ├── front/             # 00-title(封面) / 00-about / 01-author / 02-ack / 03-rev
        ├── main/              # 第 1–5 章：C++98/03 → C++11 → C++14 → C++17 → C++20
        ├── back/              # 02-references（参考文献） / 03-index（书末索引，双栏）
        └── appendix/          # 01(技术清单) / 02(C++ Lambda 表达式的六大优势)
```

> 上一层目录另有原书英文 PDF（`*.pdf`）与 `work/` 工作区（提取脚本、章节 txt、日志等，非编译所需）。

---

## 环境依赖

| 依赖 | 版本 / 说明 |
|---|---|
| TeX Live | 2026（含 `xelatex`、minted **v3** / `latexminted`、`pygmentize`） |
| Python | 3.x（提供 `pygmentize`，且 `latexminted` 脚本需要可用的 `python.exe`） |
| 中文字体 | **Source Han Serif SC（思源宋体）**、**Microsoft YaHei（微软雅黑）** |
| 拉丁字体 | Cambria、Franklin Gothic Medium、Consolas |
| 等宽字体 | Consolas（Windows 自带，无需额外安装） |

> 字体缺失会导致编译报 "font not found"。思源宋体与微软雅黑需本机已安装。

---

## 编译

本项目**使用 `build.bat`** 进行一键编译（该脚本已针对本项目正确配置，标题、路径、minted 缓存目录均已就位）：

```bash
# 在本目录（latex_source/）下，双击 build.bat，或：
cmd /c build.bat
```

`build.bat` 会自动完成：设置 PATH（Python 根目录在 TeX bin 之前）、设置 minted v3 所需的
绝对路径缓存目录（`TEXMF_OUTPUT_DIRECTORY` / `TEXMFOUTPUT`）、连跑三遍 `xelatex -shell-escape`、
扫描高亮失败、最后清理临时的 `*.minted` / `*.listing` 文件。正常退出码为 **0**，并产出 `main.pdf`。

如需手动编译（例如在 bash / WSL 下）：

```bash
# 1) 设置 PATH：Python 根目录必须在 TeX bin 之前
export PATH="/d/ProgramData/texlive/2026/bin/windows:$PATH"
export PATH="/d/anaconda3:$PATH"          # 视本机 Python 安装位置调整
# 2) 让 minted v3 把缓存写到绝对路径（见「已知坑」第 1 条），必须指向本工程绝对根
export TEXMF_OUTPUT_DIRECTORY="D:/Ego/trans/C++ Lambda Story/latex_source"
export TEXMFOUTPUT="$TEXMF_OUTPUT_DIRECTORY"

# 3) 跑三遍（目录 / 书签 / 索引页码需多遍才稳定）
xelatex -shell-escape -interaction=nonstopmode -file-line-error main.tex
xelatex -shell-escape -interaction=nonstopmode -file-line-error main.tex
xelatex -shell-escape -interaction=nonstopmode -file-line-error main.tex
```

- `-shell-escape` 是必须的（minted 调外部高亮器）。
- 首次或 `book/content/**` 有变动后，minted 会重高亮全部代码清单（约数分钟）；
  缓存命中时一遍约 1 分钟。
- 用 `-file-line-error` 可快速定位报错所在的文件与行号。
- 编译产物：`main.pdf`（约 125 页）、`main.aux`、`main.toc`、`main.log` 等。

---

## 封面与提示框

- **封面**（`book/content/front/00-title.tex`）：直接嵌入**英文原版封面图片**
  `images/cover_original.png`（由原书 PDF 第 1 页渲染）。保持原图比例，按页面高度缩放并居中，
  不拉伸、不裁切文字。较之 TikZ 重绘版式更忠实于原书，且对编译遍数无特殊要求
  （`build.bat` 仍连跑三遍以稳定目录与书签）。
- **提示框**：原书**没有**任何文字标签（NOTE/TIP/WARNING 都不存在），所谓"标签"
  就是页边图标。本项目沿用原书两个图标：
  - `images/icon_info.png`（ℹ 信息框）
  - `images/icon_warning.png`（⚠ 警告框）
  导言区提供三个无参数环境：`myInfoBox`（左侧信息图标 + 正文缩进）、
  `myWarnBox`（左侧警告图标 + 正文缩进）、`myQuote`（灰底细边框引用框，常用于引用 C++ 标准）。

---

## 正文插图说明

原书正文以代码清单、注释框、提示框为主，**真正的光栅插图很少**。已抽取并保留 5 张有内容的图：

- `fig_p028_1.png`：C++11 中的 Lambda 语法
- `fig_p094_1.png`：C++17 中的 Lambda 语法
- `fig_p126_1.png`：C++20 中的 Lambda 语法
- `fig_p147_1.png`：编译时间对比
- `fig_p148_1.png`：在源文件中的来回跳转

抽取阶段曾把若干原书页面里的占位图存成纯黑 PNG，已在整理时删除，仅保留上述有内容的图。

---

## 索引

本工程**包含原书末尾的 Index 章节**（双栏排版），位于 `book/content/back/03-index.tex`，
由 `book/index.tex` 以 `\myFront{索引}{...}` 装配。参考文献与索引是相互独立的两节
（`02-references.tex` 与 `03-index.tex`），已在整理时拆分清楚。

---

## 已知坑与约定

1. **minted v3 缓存必须绝对路径**：`latexminted` 拒绝相对路径写入，默认的 `_minted`
   缓存会全部失败（报 `Cannot write file "_xxx.index.minted" outside working directory`）。
   已在 `book/ccs.tex` 把 `\minted@cachedir` 设成 `TEXMF_OUTPUT_DIRECTORY` / `TEXMFOUTPUT`
   给出的工程绝对根；`build.bat` 会自动设置这两个变量，手动编译时也务必先 `export`。
2. **LaTeX 转义铁律**：`\texttt{}` 不改变字符类别，`_ # % &` 仍需转义；
   `\url{}` 中 `_` 直接写、`# %` 必须转义；代码体 / `minted` 内部无需转义。
3. **章节装配**：增删/调序章节只改 `book/index.tex` 里的 `\myChapter{}` / `\myAppendix{}`
   / `\myFront{}` 行即可，无需动其它文件。
4. **代码清单可选参数**：`\begin{codebox}`（无标题）与 `\begin{codeboxt}{标题}`（带标题）
   已确认可用；不要在 `codebox` 后误加方括号语言参数，否则会被当成未知语言名。

---

## 字体映射

| 用途 | 字体 |
|---|---|
| 中文正文（衬线） | Source Han Serif SC（思源宋体） |
| 中文标题（无衬线） | Microsoft YaHei（微软雅黑） |
| 拉丁正文 | Cambria（替代原书商用 New Baskerville） |
| 拉丁标题 | Franklin Gothic Medium |
| 代码 / 等宽 | Consolas |
| 代码块中的中文注释 | Microsoft YaHei |

---

## 工作流（供复核）

1. 从原书 PDF 用 `../work/extract_lambda.py` 提取章节文本（`../work/chapters/*.txt`）。
2. 对照 `../work/TRANSLATION_GUIDE.md` 翻译，产出 `book/content/**/*.tex`；
   多段章节经 `../work/assemble.py` 装配，再由 `../work/escape_special.py` 做转义后处理。
3. 按上节命令（或 `build.bat`）编译，检查 `main.log` 无报错、目视核对封面与提示框。
