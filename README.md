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


> 如果环境配置遇到问题或者编译失败可以直接到[releases](https://github.com/jnulzl/CXX-Lambda-Story-CN/releases)下载最新的pdf文件
> 
## 官方代码

[cpplambdastory-code](https://github.com/fenbf/cpplambdastory-code) —— 书中示例代码。