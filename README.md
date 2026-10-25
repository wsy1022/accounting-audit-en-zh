# 财务报表中英翻译器 Financial Statement Translator

把**英文财务报表 / 年报 / 审计报告 / 财务数据**翻译成中文，并用内置术语库锁定专业术语、自动核对数字不丢失。

面向：财务、审计、投行、跨境业务人员，以及读英文年报吃力的学生。

![界面预览](docs/screenshot.png)

仓库里有两样东西：

| 目录 | 是什么 | 给谁用 |
|---|---|---|
| `skills/accounting-audit-en-zh/` | 一份 Agent Skill（提示词工作流 + 术语库 + 翻译规则 + 155 组句式） | 用 Claude / ChatGPT / Codex / Hermes 等 AI 助手读财报的人 |
| `app/` | 可执行翻译器（GUI + 命令行），打包为 Windows exe | 想双击就用、不懂命令行的人 |

---

## 一、术语库（两处共用）

`skills/accounting-audit-en-zh/references/glossary.csv` — **750 条**：

- 629 条会计与审计核心术语（按教材五个部分：会计原理 / 财务会计 / 成本与管理会计 / 国际会计 / 审计）
- 179 条报表行项目（资产负债表、利润表、现金流量表、权益变动表、附注、审计报告、财务比率）

CSV 可直接导入 Excel / Anki / 欧路词典背单词。查询：

```bash
python skills/accounting-audit-en-zh/scripts/term_lookup.py "net realisable value"
python skills/accounting-audit-en-zh/scripts/term_lookup.py --zh 坏账准备
```

---

## 二、用 Skill（AI 助手路线）

把 `skills/accounting-audit-en-zh/` 整个目录放进你的 skills 目录（Claude Code: `~/.claude/skills/`；Hermes: `~/AppData/Local/hermes/skills/`），或直接把 `SKILL.md` 的内容贴进对话开头。

之后如果说「帮我翻这份英文资产负债表」，助手会按这套流程走：

1. 判口径（IFRS / US GAAP / CAS）与单位（千 / 百万）
2. 结构对齐成行项目表
3. 术语按库落地，不自创译名
4. 数字与合计校验
5. 交付双语对照 + 术语一致性报告 + 不确定清单

配套参考文件：

- `references/statement-layouts.md` — 四张报表 + 附注 + 审计报告的英文结构、科目对照、格式陷阱
- `references/translation-rules.md` — 数字/单位/日期/易混词判例（provision vs allowance vs reserve 等）
- `references/sentence-patterns.md` — 155 组「英文原句 → 中文」句式，整句背诵用

---

## 三、用 exe（不装 Python 路线）

到 [Releases](../../releases) 下载 `FinancialStatementTranslator.exe`，双击运行：

1. 选文件（`.docx` `.xlsx` `.txt` `.md` `.csv` `.pdf` 均可）
2. 填 API Key（默认接口是 DeepSeek，也可填 OpenAI / 通义 / 智谱 / 本地 Ollama 的 OpenAI 兼容地址）
3. 点「开始翻译」→ 输出中英对照 Word

命令行同样可用（需要 Python 3.9+）：

```bash
python app/translator.py --cli 10-K.docx -o 10-K_中文.docx --bilingual
python app/translator.py --cli balance_sheet.xlsx --zh-only
```

### 它是怎么保证「准」的

| 机制 | 作用 |
|---|---|
| 术语注入 | 只把本文命中的术语（≤80 条）送进提示词，避免长清单稀释注意力 |
| 术语达标检查 | 译完后逐条核对术语是否用了规定译法，不达标自动补救重译一次 |
| 数字校验 | 抽原文全部数字与译文比对，缺一个就报警并列出来 |
| 结构保留 | 按段落/行切块翻译，表格行不合并、条目不压缩 |

### 它做不到什么

- 不是机器翻译替代品：**必须人工复核**，尤其附注、或有事项、关联方披露这类长句。
- 不做汇率/单位换算，原文数字照搬（单位写在表头）。
- 不识别图片型 PDF（扫描件需先 OCR）。
- 不构成审计意见、税务或投资建议。

---

## 四、本地文件结构

```
accounting-audit-en-zh/
├── README.md
├── LICENSE
├── skills/accounting-audit-en-zh/
│   ├── SKILL.md
│   ├── references/
│   │   ├── glossary.csv              # 750 条术语总表
│   │   ├── glossary-statements.csv   # 179 条报表行项目（可分册维护）
│   │   ├── statement-layouts.md
│   │   ├── translation-rules.md
│   │   └── sentence-patterns.md
│   └── scripts/term_lookup.py
└── app/
    ├── translator.py                 # GUI + CLI 主程序
    ├── glossary.json                 # exe 内置术语库
    ├── requirements.txt
    ├── build_exe.bat                 # 一键打包
    └── README.md
```

---

## 五、自己打包 exe

```bash
pip install pyinstaller python-docx openpyxl pymupdf
cd app
pyinstaller --noconfirm --onefile --windowed --name FinancialStatementTranslator ^
  --add-data "glossary.json;." translator.py
```

产物在 `app/dist/`。

---

## 六、术语来源与声明

- 术语与句式参考《会计审计专业英语》（贺欣、温倩、罗殿英编著，机械工业出版社）整理，用于学习与翻译辅助；教材原文著作权归原作者与出版社。
- 报表行项目部分按 IFRS / US GAAP 常见列报习惯与《企业会计准则》中文用语对照整理。
- 术语表为公开会计用语的对照汇编，供学习参考；商用请自行核对最新准则表述。

## License

MIT（代码部分）。术语表与文档采用 CC BY 4.0。
