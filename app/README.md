# 翻译器（app）

GUI + CLI 二合一，单文件即可运行。

## 运行

```bash
pip install -r requirements.txt
python translator.py                      # 图形界面
python translator.py --cli input.docx     # 命令行
```

## 配置文件

API Key / 接口地址 / 模型保存在 `%APPDATA%\AccountingTranslator\config.json`，不随 exe 分发，也不上传。

默认：`https://api.deepseek.com/v1` + `deepseek-chat`。
换 OpenAI：`https://api.openai.com/v1` + `gpt-4o-mini`。
本地 Ollama：`http://127.0.0.1:11434/v1` + `qwen2.5:14b`（Key 随便填）。

## 支持的输入

`.txt` `.md` `.csv` `.docx` `.xlsx` `.pdf`（文本层；扫描件需先 OCR）

## 输出

按扩展名决定：`.docx` 生成双语对照表格，`.xlsx` 生成两列工作表，其他为纯文本。

## 参数

| 参数 | 说明 |
|---|---|
| `--bilingual` / `--zh-only` | 中英对照 / 只要中文（默认对照） |
| `--api-key` `--base-url` `--model` | 覆盖配置 |
| `--chunk-chars` | 每块字符数（默认 1600，越小越省 token 但连贯性下降） |
