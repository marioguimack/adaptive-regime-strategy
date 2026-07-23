# Adaptive Regime-Based Strategy with Walk‑Forward Validation

##  Model Description

The strategy is built around a **volatility regime classification model** that determines whether the market is currently in a *Calm* or *Volatile* state. Instead of predicting returns directly, the model focuses on forecasting changes in market volatility, which means a more stable and persistent signal in financial time series.

### Core Idea
Financial markets behave differently depending on volatility conditions.  
- In **Calm** regimes, trends tend to persist and momentum signals perform well.  
- In **Volatile** regimes, price movements become erratic, drawdowns increase, and defensive positioning becomes more effective.

The model identifies these regimes using historical market features and then adapts the trading strategy accordingly.

### Model Type
The classifier is a **Random Forest**, chosen for its:
- robustness to noisy financial data  
- ability to capture non‑linear relationships  
- stability across different market environments  
- resistance to overfitting due to ensemble averaging  

The output is a binary regime label:
- `0` → Calm  
- `1` → Volatile

### Input Signals
The model uses a diverse set of market‑derived features that capture trend, volatility, and market pressure, including:
- rolling volatility measures  
- momentum indicators  
- ATR‑based volatility  
- moving average divergence  
- volume‑adjusted signals  
- short‑term vs. long‑term trend structure  

These features allow the model to detect when volatility is rising or falling.

### Adaptive Behavior
The predicted regime directly controls the strategy’s behavior:
- **Calm Regime:** higher exposure, trend‑following signals, aggressive position sizing  
- **Volatile Regime:** reduced exposure, defensive allocation, risk‑off behavior  

This adaptive switching is the core of the model:  
it allows the strategy to capture upside during stable periods while protecting capital during turbulent markets.

### Why It Works?
Volatility regimes tend to persist longer than price trends.  
By predicting volatility instead of returns, the model:
- avoids noisy return prediction  
- captures structural market behavior  
- provides stable, actionable signals  
- enables dynamic risk management  

This leads to smoother performance, higher Sharpe ratios, and significantly lower drawdowns.

---

##  Methodology

### 1. Data
The strategy is built using daily OHLCV data from SPY (S&P 500 ETF).  
Key preprocessing steps include:
- Calculating daily returns  
- Computing rolling volatility (20–60 day windows)  
- Generating technical indicators such as:
  - Moving averages (SMA, EMA)
  - RSI
  - ATR
  - Rolling z‑scores
  - Momentum signals

These features form the input for regime classification and signal generation.

### 2. Regime Classification
A Random Forest classifier is trained to distinguish between **Calm** and **Volatile** market regimes.

**Training period:** 2010–2019  
**Walk‑forward prediction period:** 2020–2026

The classifier uses:
- Rolling volatility  
- Price momentum  
- ATR‑based volatility  
- Short‑term vs. long‑term trend divergence  
- Volume‑based features  

The output is a binary regime label:
- `0` → Calm  
- `1` → Volatile  

This regime prediction drives the adaptive behavior of the strategy.

### 3. Strategy Logic
The strategy switches between two behaviors depending on the predicted regime:

#### Calm Regime (0)
- Trend‑following signals  
- Higher exposure  
- Momentum‑based entries  
- Volatility‑targeted position sizing

#### Volatile Regime (1)
- Defensive allocation  
- Reduced exposure  
- Mean‑reversion or flat positioning  
- Risk‑off behavior to avoid drawdowns

This adaptive switching allows the model to capture upside during stable periods and protect capital during turbulent markets.

### 4. Walk‑Forward Backtesting
To ensure realistic performance, the strategy uses a **walk‑forward evaluation**:
- Train on past data  
- Predict the next period  
- Update the model as time progresses  
- Avoid look‑ahead bias  
- Avoid data leakage  

The backtest includes:
- Daily rebalancing  
- Transaction cost modeling  
- Slippage assumptions  
- Volatility‑adjusted position sizing  

Performance is evaluated using:
- Cumulative returns  
- Rolling Sharpe ratio  
- Maximum drawdown  
- Regime‑specific behavior analysis

This methodology ensures the strategy is robust, adaptive, and tested under realistic market conditions.

---

### Results and Interpretation

##  Walk‑Forward Performance Interpretation

The walk‑forward evaluation provides a realistic, out‑of‑sample assessment of how the strategy performs when retrained and tested across rolling market periods. The following metrics summarize the strategy’s effectiveness under frictions and volatility‑targeted position sizing:

### **CAGR: 24.44%**
A walk‑forward CAGR of **24.44%** indicates strong and consistent long‑term growth.  
This means the strategy compounds capital at more than **double** the historical return of the S&P 500, even after accounting for transaction costs and adaptive position sizing.  
Such a high CAGR in walk‑forward testing suggests that the model generalizes well across different market regimes.

### **Max Drawdown: –3.77%**
A maximum drawdown of only **–3.77%** is exceptionally low for an equity‑based strategy.  
This demonstrates that the regime classifier and volatility‑aware sizing effectively reduce exposure during turbulent periods, preventing large losses.  
The shallow drawdown profile confirms strong downside protection and stable risk management.

### **Sharpe Ratio: 3.39**
A walk‑forward Sharpe ratio of **3.39** is unusually high for an out‑of‑sample strategy.  
This indicates that returns are generated very efficiently relative to risk.  
A Sharpe above 3 suggests:
- consistent performance across regimes  
- low volatility of returns  
- strong alignment between predicted regimes and actual market behavior  

Overall, these metrics show that the strategy is **high‑return, low‑risk, and highly stable**, even under realistic walk‑forward conditions.

---

### Adaptive Strategy Returns with Predicted Volatility Regimes
![Adaptive Strategy Returns](plots/Strategy%20returns%20with%20predicted%20volatility%20regimes.png)

This figure compares the cumulative returns of the adaptive strategy (blue line) against a Buy‑and‑Hold SPY benchmark (gray dashed line), while overlaying volatility regimes predicted by the model.

**Interpretation:**
- The adaptive strategy grows from ~1 to above 10× cumulative return, far outperforming SPY (~4×).
- The blue line’s steady upward trajectory shows strong compounding and effective regime adaptation.
- The red markers (Volatile) and blue markers (Calm) indicate the model’s classification of market conditions.
- During calm regimes, the strategy tends to accelerate, taking advantage of stable trends.
- During volatile regimes, returns flatten or slow, reflecting defensive positioning and reduced exposure.

This visualization highlights the model’s ability to adjust exposure dynamically based on volatility forecasts, achieving superior long‑term growth while controlling risk.

### Rolling Sharpe Ratio (60‑Day Window)
![Rolling Sharpe Ratio](plots/Sharpe%20ratio(60-day%20window).png)

This plot tracks the risk‑adjusted performance of the adaptive strategy over time.
The Sharpe ratio measures how much excess return the strategy generates per unit of risk.

**Interpretation:**
- The ratio fluctuates between roughly ‑4 and +7, showing alternating periods of strong and weak performance.
- High peaks (above 4) indicate phases of exceptional risk‑adjusted returns, where the strategy captured  favorable market conditions efficiently.
- Deep troughs (below 0) correspond to periods of instability or losses relative to volatility.
- The cyclical pattern suggests that the model’s effectiveness varies with market regimes — performing best in calm or trending environments and weakening during turbulent transitions.

This chart demonstrates that the strategy maintains consistently positive Sharpe ratios most of the time, implying robust risk management and adaptability.


### Strategy Drawdown (Walk‑Forward)
![Strategy Drawdown](plots/Strategy%20drawdown.png)

This chart shows the percentage decline from the strategy’s peak value over time — a direct measure of downside risk.

**Interpretation:**
- Drawdowns remain mostly between ‑1% and ‑3.5%, indicating limited capital erosion even during adverse periods.
- Frequent small dips reflect normal market fluctuations, while deeper troughs mark stress events (e.g., volatility spikes).
- The absence of prolonged deep drawdowns suggests that the adaptive regime model effectively mitigates risk through position sizing and volatility awareness.

Overall, the drawdown profile confirms that the strategy maintains strong capital preservation, complementing its high cumulative returns.

---

##  Model Limitations

Although the volatility‑based regime classifier improves stability and reduces drawdowns, the model has several important limitations:

### 1. Sensitivity to Feature Engineering
The Random Forest relies heavily on hand‑crafted technical features.  
If these features fail to capture emerging market dynamics, the model may misclassify regimes or react too slowly to structural changes.

### 2. Limited Regime Complexity
The model predicts only two regimes: *Calm* and *Volatile*.  
Real markets exhibit richer behavior (e.g., trending, mean‑reverting, liquidity‑driven, macro‑driven).  
Reducing market structure to two states oversimplifies reality and may miss nuanced transitions.

### 3. Lag in Regime Detection
Volatility often spikes suddenly.  
Because the model uses rolling windows and historical features, regime shifts may be detected **after** they occur, causing delayed defensive reactions.

### 4. No Awareness of Macro or Cross‑Asset Signals
The classifier uses only SPY‑based technical indicators.  
It does not incorporate:
- VIX  
- yield curve signals  
- credit spreads  
- macroeconomic releases  
- cross‑asset volatility spillovers  

This limits its ability to anticipate regime changes driven by broader market forces.

### 5. Walk‑Forward Stability Depends on Market Conditions
The model performs well in trending or moderately volatile markets, but may struggle during:
- regime‑switching clusters  
- flash crashes  
- liquidity shocks  
- macro‑driven reversals  

Performance is therefore **market‑regime dependent**, not universally stable.

### 6. Random Forest Interpretability
Although Random Forests are more interpretable than deep learning models, they still act as black‑box ensembles.  
Understanding *why* a specific regime was predicted is not always straightforward.

### 7. No Direct Return Prediction
The model predicts volatility regimes, not returns.  
This avoids noise but also means the strategy cannot exploit return‑specific opportunities that are unrelated to volatility.

These limitations highlight areas for future improvement, such as adding macro features, expanding regime categories, or experimenting with more expressive models like gradient boosting or LSTMs.
