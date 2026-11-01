# V9 Sniper: Mathematical Strategy Logic

This document outlines the core mathematical logic and microstructure concepts used in the V9 Sniper Strategy. The goal of this research was to replace lagging retail indicators (ADX, CHOP, Volume SMAs) with pure price action, volatility gates, and momentum architecture.

## 1. The Trigger (Momentum Ignition)
**Concept:** We look for a sudden, aggressive injection of buying or selling pressure.
**Math:** RSI(14) change in a single candle > 10 points.
*   *Long:* `RSI - RSI[1] > 10`
*   *Short:* `RSI - RSI[1] < -10`

## 2. The Structure (EMA Alignment)
**Concept:** We do not need a perfect stacked ribbon (which is too rare). We only need the short-term momentum (EMA 20) to align with the macro trend.
**Math:** 
*   *Long:* `EMA_20 > EMA_50` AND (`EMA_20 > EMA_75` OR `EMA_20 > EMA_100` OR `EMA_20 > EMA_150`)
*   *Short:* `EMA_20 < EMA_50` AND (`EMA_20 < EMA_75` OR `EMA_20 < EMA_100` OR `EMA_20 < EMA_150`)

## 3. The ATR Gate (The Crown Jewel)
**Concept:** A spike in a dead, tangled market is a trap. We demand that the market is tightly packed and highly volatile.
**Math:** We calculate the True Range volatility buffer.
*   *Long:* `Spike Low - ATR(14)` MUST drop strictly below **ALL 6 EMAs** (EMA 5, 20, 50, 75, 100, 150).
*   *Short:* `Spike High + ATR(14)` MUST rise strictly above **ALL 6 EMAs**.

## 4. The Entry (20% Breakout Confirmation)
**Concept:** Do not buy the initial spike (FOMO). Wait for the market to prove it has continuation power.
**Math:** 
*   Calculate the Spike Range (`High - Low`).
*   *Long Limit Order:* `Spike High + (Spike Range * 0.20)`
*   *Short Limit Order:* `Spike Low - (Spike Range * 0.20)`

## 5. The "Candle 2" Trap Avoidance
**Concept:** Market makers often use the immediate candle after a spike to hunt liquidity, creating massive wicks. We wait for Candle 2 to close before placing the limit order.
**Math:** If Candle 2 is the opposite color of the spike, AND its body is `> 50%` of the spike body, the setup is canceled entirely.

## 6. Risk Management (The SAR Flip Anchor)
**Concept:** The Parabolic SAR (PSAR) is the structural anchor of the trend. If the price falls back to where the SAR flipped, the trend is dead.
**Math:** 
*   *Stop Loss:* The exact price of the First SAR Flip that started the current trend.
*   *Hard Filter:* If the distance from Entry to SAR Flip is `> 1.5%`, skip the trade (the Risk/Reward is mathematically broken).

## 7. Exit Strategy (Dynamic Scale-Out)
*   **0.5R:** Move Stop Loss to Breakeven.
*   **If initial SL < 0.7%:** 30% out at 0.75R, 40% out at 1.0R, 30% runner at 1.5R.
*   **If initial SL > 0.7%:** 20% out at 0.5R, 30% out at 0.75R, 30% out at 1.0R, 20% runner at 1.5R.
