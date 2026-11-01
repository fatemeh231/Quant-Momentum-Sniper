# V9 Sniper: Quantitative Momentum Ignition & ATR Gate Strategy

## 📌 Introduction
The V9 Sniper is a quantitative trading framework designed to capture the exact "ignition phase" of a trend in highly volatile markets (specifically tested on BTC/USDT 5-minute charts). 

Unlike traditional retail strategies that rely on heavily lagged oscillators (e.g., ADX, CHOP, Volume SMAs), this project pivots to **Market Microstructure**, **EMA Ribbon Alignment**, and **Volatility Filtering**. By eliminating lagging indicators and applying strict mathematical gates, the strategy successfully transformed a losing setup (28% Win Rate, Profit Factor 1.1) into a high-probability edge (**59.6% Win Rate, Profit Factor 1.65**).

**Note on Intellectual Property:** The full Python backtesting pipeline, the Machine Learning Random Forest model, and the raw 81-trade manually labeled dataset are kept **private** as proprietary intellectual property. The `screenshots` folder provides visual verification of the research and backtest results. The included Pine Script is the visual execution layer.

---

## 🛠️ Tech Stack
*   **Data Engineering & Backtesting:** Python (Pandas, NumPy, CCXT for Binance API data extraction).
*   **Machine Learning:** Scikit-Learn (Random Forest Classifier, Feature Importance, GridSearch).
*   **Visualization & Execution:** Pine Script (v6) for TradingView chart integration.
*   **Statistical Analysis:** Expectancy, Profit Factor, MFE/MAE (Max Favorable/Adverse Excursion), Kelly Criterion.

---

## 🚨 The Core Challenges & How We Overcame Them

### Challenge 1: The "Lagging Indicator" Trap
**Problem:** The initial strategy used 36 indicators. The backtest showed a negative Profit Factor because indicators like ADX and CHOP were reacting 70+ minutes after the initial momentum spike.
**Solution:** We ran an Ablation Study (removing features one by one) and discovered that deleting ADX, CHOP, and Volume actually *improved* the edge. We replaced them with a strict **EMA 20 Alignment** rule (EMA 20 must be aligned with at least one macro EMA: 75, 100, or 150).

### Challenge 2: The "SL > 2%" Death Spiral
**Problem:** Using the Parabolic SAR (PSAR) Flip as a stop-loss often resulted in a massive Risk (R0) distance. If a 20% breakout occurred but the SAR was 3% away, the 1.5R target was mathematically unreachable on a 5-minute chart.
**Solution:** We introduced a hard filter. If the distance between the entry and the SAR flip is **> 1.5%**, the trade is automatically rejected.

### Challenge 3: The Candle 2 "Shadow" Trap
**Problem:** Market makers often use the immediate candle after a spike to hunt liquidity (creating massive wicks), trapping early breakout traders.
**Solution:** We implemented a **Candle 2 Rejection Rule**. We wait for Candle 2 to close. If Candle 2 is an opposite-colored candle with a body > 50% of the original spike's body, the trade is invalidated (canceling the limit order).

### Challenge 4: The "Cliff of Death" (EMA Cross Age)
**Problem:** Entering on spikes that happened too late in a trend resulted in a 0% win rate.
**Solution:** Through manual labeling, we discovered the edge completely disappears if the EMA 20/50 cross is older than **7 candles** (35 minutes). The Pine Script enforces this freshness.

---

## 📊 The 3-Pillar Validation Logic

1.  **Momentum Ignition:** A single-candle RSI spike of >10 points (changes in raw RSI momentum).
2.  **Structural Alignment:** EMA 20 > EMA 50, AND EMA 20 > (EMA 75, 100, or 150).
3.  **The ATR Gate (Crown Jewel):** `Spike Low - ATR(14)` must drop strictly below **all 6 EMAs**. This mathematically proves the trend is tightly packed, volatile, and not overextended.

---

## 📈 The Pine Script Visualizer
The `pine_script` folder contains the V9 visualizer. When added to TradingView, it provides:
*   **The EMA Ribbon:** 6 EMAs (5, 20, 50, 75, 100, 150) and Parabolic SAR.
*   **Cross Markers:** Visual `+` signs for EMA 20/50 and 5/150 crosses.
*   **Sniper Arrows:** Large Green/Red arrows appear *only* when all 3 Pillars align perfectly.
*   **Smart Labels:** Automatically prints the exact ATR value and the 20% Breakout Limit Price for execution.

## 🚀 About the Author
I am a Computer Science student and Quantitative Developer specializing in Algorithmic Trading, Machine Learning, and Data Engineering. I build end-to-end trading systems, from data ingestion and feature labeling to ML backtesting and Pine Script deployment. I am currently also serving as a Project Ambassador & Market Analyst for Toobit.

*   **GitHub:** [github.com/fatemeh231](https://github.com/fatemeh231)
*   **LinkedIn:** [Seyedeh Fatemeh Hosseininasab](https://www.linkedin.com/in/seyedeh-fatemeh-hosseininasab-7320bb322/)
*   **X (Twitter):** [@fatemeh1299](https://x.com/fatemeh1299)
*   **TradingView:** [Fatemeh-Hosseini-Nasab1](https://www.tradingview.com/u/Fatemeh-Hosseini-Nasab1/)

*Disclaimer: This tool is for educational and quantitative research purposes only. It does not constitute financial advice.*