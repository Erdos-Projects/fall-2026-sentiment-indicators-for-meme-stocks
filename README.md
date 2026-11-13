# 📈 Sentiment Indicators for Meme Stocks

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Erdos-Projects/fall-2026-sentiment-indicators-for-meme-stocks/blob/main/00_START_HERE_collaboration.ipynb)
![Status](https://img.shields.io/badge/status-completed-brightgreen)
![Python](https://img.shields.io/badge/python-3.x-blue)
![Erdős Institute](https://img.shields.io/badge/Erd%C5%91s%20Institute-Fall%202026-orange)

Deep-learning sentiment indicators for **GME**, **AMC** and **BB**, built from financial news and r/wallstreetbets comments.

Built by **Bianca Li, Keng Lin, Bahareh Baharinezhad** · [The Erdős Institute](https://www.erdosinstitute.org/) Deep Learning Project · Oct 1 – Nov 13, 2026

---

## ✨ Highlights

- Scores text from **-1 (bearish)** to **+1 (bullish)** across ~900k Reddit comments, 8.8k news articles and 9.5k tweets.
- **Teacher–student pipeline:** Qwen2.5-7B labels a subset, and a lightweight **BiLSTM** learns from it.
- BiLSTM matches the LLM on human-labeled tweets (**0.824 accuracy, 0.776 macro-F1**), with ~2,400× fewer parameters.
- Labels ~913k Reddit comments in **under a minute** on a T4 GPU (the LLM needs ~97 GPU-hours).
- Daily indicators (sentiment, ambiguity, **QISS**) track trading volume and volatility, but do not predict returns.

## 🚀 Quick Start (Google Colab)

The easiest way to run the project is in Google Colab. Start with **`00_START_HERE_collaboration.ipynb`**.

<!-- TODO: confirm these steps against 00_START_HERE_collaboration.ipynb -->

1. **Open the notebook** with the *Open in Colab* badge above, or from the shared [Google Drive folder](https://drive.google.com/drive/folders/1j1t_O_RLjhxA_K9bmiKgBba4uP45zc_H).
2. **Make your own copy:** *File → Save a copy in Drive*, so your edits don't affect teammates.
3. **Pick a runtime:** *Runtime → Change runtime type → T4 GPU* (needed for LLM labeling; the BiLSTM also runs on CPU).
4. **Connect to the shared folder:** run the setup cells at the top of the notebook (e.g. mounting Google Drive) and approve access when prompted.
5. **Run cells top to bottom** and follow the notebook's instructions for the next step.

## 🧠 How It Works

```
News / Tweets / Reddit  →  Clean & align to trading days  →  LLM teacher labels a subset
        →  BiLSTM student trained on those labels  →  Score all text
        →  Daily indicators (sentiment, ambiguity, QISS)  →  Compare with volume & volatility
```

| Component | What it does |
|---|---|
| Keyword baseline | Loughran–McDonald dictionary counts (0.504 macro-F1) |
| Teacher | Qwen2.5-7B-Instruct, calibrated on human-labeled tweets (0.769 macro-F1) |
| Student | BiLSTM with Word2Vec embeddings, ~3.2M parameters (0.776 macro-F1) |
| QISS | Sentiment score relative to its recent baseline, to flag unusual sentiment |

## 📊 Data

| Source | Tool | Size |
|---|---|---|
| Financial news (2009–2023) | FNSPID | 8,784 articles |
| Finance tweets | Twitter Financial News Sentiment | 9,543 tweets |
| r/wallstreetbets (Nov 2020 – Jul 2021) | Arctic Shift API | ~913,000 comments |
| Daily prices | `yfinance` | GME, AMC, BB |

## 🔍 Key Findings

- Text volume relates to next-day volatility (Reddit comment count: r = 0.63).
- Reddit sentiment relates to next-day abnormal volume (r = 0.42).
- News sentiment shows **no** relationship with next-day returns.

> Findings are preliminary and correlational. Formal tests of predictive value are future work.

## 🛣️ Future Work

Better handling of Reddit slang and emojis (🚀💎🙌), longer Reddit coverage, improved label quality, hyperparameter tuning, and formal statistical testing.

## 📚 Resources

- [Erdős Institute project database](https://www.erdosinstitute.org/project-database)
- [Google Drive / Colab folder](https://drive.google.com/drive/folders/1j1t_O_RLjhxA_K9bmiKgBba4uP45zc_H)

## 🙏 Acknowledgments

- Thanks to the [Erdős Institute](https://www.erdosinstitute.org/) for the boot camp and project framework.
- **Claude** ([Anthropic](https://www.anthropic.com/)) helped draft and format this README. Claude Sonnet 5.5 was also one of the LLMs used to cross-check sentiment labels on Reddit comments.

## ⚠️ Disclaimer

For educational and research purposes only. This is not financial advice.
