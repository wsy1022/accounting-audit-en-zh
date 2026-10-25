<p align="center">
  <img src="docs/banner.svg" alt="Financial Statement Translator — 财务报表中英翻译器" width="1000">
</p>

<p align="center">
  <a href="https://github.com/wsy1022/accounting-audit-en-zh/actions/workflows/build.yml"><img src="https://github.com/wsy1022/accounting-audit-en-zh/actions/workflows/build.yml/badge.svg" alt="build"></a>
  <a href="https://github.com/wsy1022/accounting-audit-en-zh/releases"><img src="https://img.shields.io/github/v/release/wsy1022/accounting-audit-en-zh?label=release" alt="release"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT%20%2B%20CC%20BY%204.0-blue" alt="license"></a>
</p>

<p align="center">
  把英文财报读成中文：术语用同一张表钉死，数字逐个对得上，报表逐行不串。<br>
  给财务、审计、跨境业务的人用，也给读英文年报吃力的学生用。
</p>

---

## 它解决的不是"翻译"，是"翻得能对账"

机翻英文报表，错的从来不是句子，是三个地方：

- **术语飘**——同一份表里 `provision` 一会儿"准备"一会儿"预计负债"，审阅的人无法判断这是不是同一个东西；
- **数字丢**——长表格里一个括号负数被吃掉，整张表就不平了；
- **行错位**——科目 A 的中文跑到科目 B 的位置上，越看越乱。

这个仓库用四道闸把这三件事按住：术语表注入、译后术语达标检查、数字差集校验、报表型输入逐行对齐（`L编号|` 协议）。

## 三种用法，选一个

| 你是什么人 | 用哪条路 | 怎么开始 |
|---|---|---|
| 想双击就用 | Releases 里的 `FinancialStatementTranslator.exe` | 下载 → 选文件 → 填 API Key → 开始翻译 |
| 想让 AI 助手替你读 | `skills/accounting-audit-en-zh/` | 把整个目录放进 skills 目录，然后说"帮我翻这份英文资产负债表" |
| 要在脚本里批量跑 | 命令行 | `python app/translator.py --cli 10-K.docx -o 10-K_中文.docx` |

支持 `txt / md / csv / docx / xlsx / pdf`（文本层）；输出中英对照 Word，或按扩展名给 Excel、纯文本。

## 术语库

| 科目 | 数量 |
|---|---:|
| 教材术语 Textbook terms | 629 |
| 报表行项目 Statement items | 179 |
| 重复项合并 Duplicates merged | (58) |
| **术语合计 Total terms** | **750** |

629 + 179 − 58 = 750，一行不多、一行不重。另外还有 **155 组**中英句式（`sentence-patterns.md`），按整句背比背单词划算。

```bash
python skills/accounting-audit-en-zh/scripts/term_lookup.py "net realisable value"
python skills/accounting-audit-en-zh/scripts/term_lookup.py --zh 坏账准备
```

CSV 可直接导进 Excel、Anki、欧路词典。

## 四道闸

| 机制 | 拦住什么 |
|---|---|
| 定向术语注入 | 只把本文命中的术语（≤80 条）送进提示词，长清单不会稀释注意力 |
| 术语达标检查 | 译完逐条核对规定译法，未达标就补救重译一次，仍不达标则报告 |
| 数字差集校验 | 抽原文全部数字与译文比对，缺一个就列出来 |
| 逐行对齐 | 判定为报表型输入时改用 `L编号\|` 协议，一行对一行，不串行 |

## 界面

<p align="center">
  <img src="docs/screenshot.png" alt="翻译器界面" width="880">
</p>

## 它做不到什么

- **不能替代人复核**。附注、或有事项、关联方披露这类长句，仍要懂中英双语会计的人过一遍。
- 不做汇率换算、不做单位缩放——数字原样照搬，单位写在表头。
- 扫描版 PDF 要先 OCR，本工具只吃文本层。
- 不构成审计意见、税务或投资建议。

## 仓库结构

```
accounting-audit-en-zh/
├── skills/accounting-audit-en-zh/
│   ├── SKILL.md                        工作流 + 硬规则 + 交付模板
│   ├── references/
│   │   ├── glossary.csv                750 条术语总表
│   │   ├── glossary-statements.csv     179 条报表行项目（单独维护）
│   │   ├── statement-layouts.md        四张表 / 附注 / 审计报告的结构
│   │   ├── translation-rules.md        数字·单位·易混词判例
│   │   └── sentence-patterns.md        155 组中英句式
│   └── scripts/term_lookup.py          命令行查词（中英双向）
├── app/
│   ├── translator.py                   GUI + CLI
│   ├── glossary.json                   exe 内置术语库
│   └── build_exe.bat                   一键打包
├── docs/                               banner.svg · screenshot.png
├── examples/                           示例利润表 + 真实输出
└── .github/workflows/build.yml         CI 自动打包
```

## 自己构建

```bash
pip install pyinstaller python-docx openpyxl pymupdf
cd app
pyinstaller --noconfirm --onefile --windowed --name FinancialStatementTranslator \
  --add-data "glossary.json;." translator.py
```

产物在 `app/dist/`。不想构建就用 CI：push 到 `main` 会自动打包上传 artifact，打 `v*` tag 会直接挂到 Release。

## 术语来源与授权

- 术语与句式参考《会计审计专业英语》（贺欣、温倩、罗殿英编著，机械工业出版社）整理，用于学习与翻译辅助；教材著作权归原作者与出版社。
- 报表行项目按 IFRS / US GAAP 常见列报习惯，对应《企业会计准则》中文用语。
- 代码 MIT，文档与术语表 CC BY 4.0。

<p align="center">
  <sub><a href="README.en.md">English</a> · <b>中文</b></sub>
</p>
