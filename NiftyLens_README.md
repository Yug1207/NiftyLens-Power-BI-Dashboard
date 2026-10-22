# NiftyLens: Nifty 50 Market Intelligence Dashboard

A Power BI dashboard that turns three years of daily Nifty 50 price data into one easy question: **how healthy is the Indian stock market today, and how healthy is any single stock in it?**


## What this project is

Most market dashboards show prices. NiftyLens shows **condition**. It takes raw daily prices for the Nifty 50 index and its member stocks and builds 30+ derived metrics on top, then rolls them into a small set of daily scores that anyone can read at a glance.

The data flow is simple:

1. **Fetch:** daily prices are pulled from Yahoo Finance with the `yfinance` Python library.
2. **Store:** the data is stored in SQL Server.
3. **Model:** Power BI connects to it and the derived metrics are built (moving averages, RSI, volatility, breadth, concentration and more).
4. **Show:** two report pages present the results, and the same report is embedded in the web app.

## Page 1: Market Health Overview

A snapshot of the Nifty 50 as a whole.

- **Five daily scores:** Health Score, Breadth Score, Trend Score, Volatility Score, and the share of stocks trading above their 20-day moving average.
- **Market Health Score over time:** the full three-year trend of the Health Score.
- **Market Participation:** a donut chart of how many stocks gained vs lost on the selected day.
- **Market Concentration:** how much of the day's total market movement came from just the top five movers.
- **Market Breadth Indicators:** breadth score and the share of stocks above their 20-day average, over time.
- **Date slicer** to pick the day or period you want to look at.

## Page 2: Individual Stock Overview

Pick any of the 50 stocks from the slicer and see its full picture.

- **Latest price** with the day's price change.
- **Candlestick chart** (open, high, low, close) with 20-day and 50-day moving average lines.
- **Price trend with moving averages**, showing close price against the MA20 and MA50.
- **Risk vs Reward:** volatility plotted against average reward over time.
- **RSI gauge** to show whether momentum looks stretched.
- **Health Score gauge.**
- **Valuation label** that reads "Overvalued" or "Not Overvalued" for the latest date.

## What the data shows

These numbers come straight from the data model inside the report file.

- **Coverage:** 37,103 daily rows covering **51 symbols** (50 stocks plus the NIFTY index) across **740 trading days**, from **17 March 2023 to 17 March 2026**. Stocks are tagged with 10 sectors, plus an "Index" label for the NIFTY series.
- **How the Health Score is built:** in the data, the daily Health Score works out to exactly **40% Breadth Score + 30% Volatility Score + 30% Trend Score**.
- **The score is steady, not jumpy:** it ranged from about **0.21 to 0.62**, with an average of about **0.40**. Yearly averages barely moved (about 0.39 in 2023, 2024 and 2025, and 0.40 so far in 2026).
- **Best and worst days:** the highest reading was on **17 April 2025** and the lowest on **12 November 2024**.
- **Latest reading:** about **0.47** on 17 March 2026, made up of a breadth score of about 0.61, a trend score of about 0.45 and a volatility score of about 0.31.
- **Overvaluation is rare by this rule:** fewer than **1%** of stock-days were flagged as overvalued.

## How the numbers are built

A few of the report's calculations are written as DAX measures:

- **Last Price:** the closing price on the last date in the current selection.
- **Price Change and Price Change %:** the last close compared with the previous day's close.
- **Is Overvalued:** reads the overvaluation flag for the latest date and shows "Overvalued" if it is 0.5 or higher.

The data model also holds tables for direction (daily return and reward), trend (MA20, MA50 and distance from each), volatility, volume (20-day average and ratio), overvaluation (RSI, z-score, distance from MA50), market drivers (daily top movers), concentration, breadth and the combined health score.

## Limits you should know about

- **This is a data-visualisation project, not investment advice.** The scores are custom indicators built for this project, not industry-standard ratings.
- **The overvaluation label is a simple rule-based flag.** It does not account for earnings, fundamentals or news.
- **Prices come from Yahoo Finance through `yfinance`,** which is a free, unofficial source. Small gaps or revisions are possible.
- **Three years of history** is enough to see trends but not enough to judge long market cycles.

## Built with

- **Python** and **yfinance** for data collection
- **SQL Server** for storage
- **Power BI** (with DAX measures, a custom light theme, and the OKViz Candlestick custom visual) for the dashboard
- **React, HTML and CSS** with **Vercel** for the web application that embeds the report

## Project structure

```
├── Dashboard_NiftyLens.pbix    # The Power BI report (both pages and the data model)
└── README.md
```

## Open it on your own computer

1. Install **Power BI Desktop** (free, Windows only).
2. Download `Dashboard_NiftyLens.pbix` from this repo.
3. Open the file. The data is already loaded in the report, so both pages work straight away.



