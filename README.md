# Cryptocurrency Price Direction

HSLU team coursework in Data Collection, Integration and Preprocessing (CIP02, group 103). We tested whether yesterday’s market indicators could predict whether today’s BTC, XRP or ICP candle would close above its open. Accuracy and recall tell different stories on the held-out dates.

The final team pipeline uses CoinMarketCap. Tharrmeehan built a separate [CoinGecko pipeline](https://github.com/raphaelspgll/CIP_FS2026_103/pull/1) and [contributed to the final report and README](https://github.com/raphaelspgll/CIP_FS2026_103/pull/17).

## Replay the models offline

Use Python 3.14 and the pinned dependencies. Run from the root of a separate checkout if you need to preserve the checked-in figure.

```sh
python -m venv .venv
. .venv/bin/activate
python -m pip install -r requirements.txt
python src/models.py
```

The script reads the three CSVs in `data/processed/`, prints validation and test metrics, and replaces `images/model_results.png`. It does not fetch data. `src/coinmarket_scrape.py` is the historical acquisition script; it requires Playwright and a browser and has not been re-verified against the live site.

## Data and evaluation

The CoinMarketCap snapshot spans 2022-12-08 to 2026-03-19, with 1,198 rows per coin before lagging. `src/features.py` defines the target as `close > open`. The five predictors are the prior candle’s close-to-close log return, close/open log ratio, high/low log ratio, seven-day rolling volatility and log volume change. Before fitting logistic regression and a decision tree, `src/models.py` lags predictors within each coin and splits the rows chronologically into 70% training, 15% validation and 15% test.

The original submission reported 62.6% accuracy for ICP logistic regression, but just 14.1% recall on up candles. The Decision Tree has no fixed seed in this version, so its results can vary between runs. A reference line on the original chart is not a demonstrated accuracy ceiling. This project did not test a trading strategy.

The [team report](latex/main.pdf), [EDA notes](docs/eda_report.md) and tracked figures record the original submission. The report uses different target wording; the committed feature tables and `src/features.py` show the label used by this script.

## AI Disclosure

The team's AI disclosure is preserved in [docs/ai_disclosure.md](docs/ai_disclosure.md).

## Authors

Krishnathasan Tharrmeehan  
Lemma Emanuel  
Spagolla Raphaël  

Hochschule Luzern (HSLU)  
MSc Applied Information and Data Science
