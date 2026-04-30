# hf-paper-skill

HuggingFace Papers 热门论文获取与中文翻译 — Hermes Agent skill

## 功能

- 获取 HuggingFace Papers 热门论文（日/周/月）
- 通过 DeepLX API 自动将论文摘要翻译为中文
- 中英文摘要对照展示
- 趋势分析 + 推荐阅读排序

## 配置

### 依赖

```bash
pip3 install python-dotenv
```

### DeepLX Token / OpenAI LLM

翻译功能支持两种后端，通过 `.env` 配置：

**DeepLX（默认）：**
```env
TRANSLATE_BACKEND="deeplx"
DEEPLX_URL="https://api.deeplx.org/你的token/translate"
```

**OpenAI 兼容 LLM（OpenAI、硅基流动、DeepSeek 等）：**
```env
TRANSLATE_BACKEND="openai"
OPENAI_BASE_URL="https://api.openai.com/v1"
OPENAI_API_KEY="sk-your-key-here"
OPENAI_MODEL="gpt-4o-mini"
```

> 主后端失败时自动降级到另一后端，都失败则保留英文原文。

## 使用方法

```bash
# 获取论文列表（今日热门）
python3 scripts/get_papers.py

# 生成分析报告（本周热门，自动翻译为中文）
mkdir -p reports
python3 scripts/generate_report.py --period weekly --output reports/HuggingFace_本周热门论文_$(date +%Y-%m-%d).md

# 指定数量和范围
python3 scripts/generate_report.py --period weekly --limit 10

# 纯英文输出
python3 scripts/generate_report.py --period weekly --no-translate
```

## 参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `--period` | daily / weekly / monthly | weekly |
| `--limit` | 论文数量 | 10 |
| `--output` | 输出文件路径 | stdout |
| `--no-translate` | 不翻译为中文 | false |

## 翻译

使用 DeepLX API 自动翻译摘要，单条翻译失败时自动降级保留英文原文。

## Hermes Agent 集成

设为 Hermes Agent skill 后，用户说"最近有什么热门论文？"即可自动调起此工具。

```bash
cp -r hf-paper-skill ~/.hermes/skills/research/hf-papers/
```
