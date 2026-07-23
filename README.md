**Adaptive Regime-Based Strategy with Walk‑Forward Validation**

This project implements a machine‑learning‑driven trading strategy that adapts to market volatility regimes.
It uses walk‑forward validation, Random Forest classification, momentum/Sharpe hybrid signals, market frictions, and volatility targeting.

| Metric | Value |
| --- | --- |
| **CAGR** | **24.44%** |
| **Max Drawdown** | **−3.77%** |
| **Sharpe Ratio** | **3.39** |


These results include:

- Realistic transaction costs
- Slippage
- Delayed signals
- Volatility targeting

**Methodology**

1. Walk‑Forward Validation
The model is retrained each year using the previous 5 years of data:

         Train: (year−5) → (year−1)     
         Test:  year
         This prevents look‑ahead bias and simulates real‑time model deployment.

3. Regime Classification
A Random Forest classifier predicts high‑volatility vs low‑volatility regimes using volatility, returns and volume.
The top 30% volatility quantile defines the “volatile” class.

3. Adaptive Signal Logic
| Regime | Signal |
| --- | --- |
| **Calm** | 10‑day momentum |
| **Volatile** | 20‑day rolling Sharpe |

4. Backtesting
Positions are generated from signals:

        Position = Signal > 0
        Strategy_Returns = Position × Returns
   
6. Market Frictions
Includes:
- 0.05% per trade
- Slippage proportional to volatility
- Delayed signals (1‑day lag)

6. Volatility Targeting
Position size scales inversely with rolling volatility:

       Position_Size = TargetVol / RollingVol
