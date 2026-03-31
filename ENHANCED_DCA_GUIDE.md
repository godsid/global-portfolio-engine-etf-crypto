# 📊 Enhanced Dynamic DCA Feature Guide

## Overview
Enhanced Dynamic DCA is a sophisticated Dollar Cost Averaging strategy that automatically adjusts investment amounts based on real-time market conditions using quantitative indicators.

## Features Added

### 1. **UI Controls** (index.html - Money Management Section)
- ✅ Enable/Disable checkbox for Enhanced DCA
- ✅ Strategy selector (4 strategies available)
- ✅ Min/Max multiplier controls (0.5x - 2.0x default)
- ✅ Lookback period configuration
- ✅ Interactive help tooltips

### 2. **Four Investment Strategies**

#### RSI-Based Strategy
**Best for:** Systematic buy-low, sell-high approach
- Increases investment when RSI < 30 (oversold)
- Decreases investment when RSI > 70 (overbought)
- Neutral adjustment between 30-70

**Use Case:** Conservative investors who want to buy dips automatically

---

#### Volatility-Based Strategy
**Best for:** Opportunity seekers during market turbulence
- Increases investment during high volatility periods
- Reduces investment when markets are calm
- Based on statistical volatility measurement

**Use Case:** Aggressive investors comfortable with market volatility

---

#### MA Crossover Strategy
**Best for:** Trend followers
- Increases investment when price is below moving average (discount)
- Decreases investment when price is above moving average (premium)
- Smooth signal based on price position relative to MA

**Use Case:** Value investors who buy at discounts

---

#### Momentum-Based Strategy (Contrarian)
**Best for:** Counter-trend investors
- Maximum investment during strong downtrends (-10%+)
- Minimum investment during strong uptrends (+10%+)
- "Buy the dip" philosophy

**Use Case:** Long-term investors who believe in mean reversion

---

### 3. **Technical Implementation**

#### New Functions Added:
```javascript
// RSI Calculation
calculateRSI(prices, period = 14)

// Dynamic multiplier calculation
calculateDCAMultiplier(cfg, benchmarkPrices, currentIdx)
```

#### Enhanced DCA Flow:
```
1. Read base DCA amount from config
2. Calculate market indicator (RSI/Vol/MA/Momentum)
3. Compute multiplier based on strategy
4. Apply bounds (Min/Max multiplier)
5. Execute DCA with adjusted amount
6. Log transaction with multiplier info
```

### 4. **Visual Indicators**

#### Transaction Log Enhancements:
- 🟢 Green badge: Increased investment (multiplier > 1.0)
- 🔴 Red badge: Decreased investment (multiplier < 1.0)
- Shows: Base amount × Multiplier = Actual amount
- Displays percentage (e.g., "150% (×1.50)")

#### Example Log Entry:
```
Date: 2025-03-15
Injection: +$1,500 [🟢 150% (×1.50)]
Base: $1,000 × 1.50 = $1,500
```

---

## How to Use

### Step 1: Configure Base DCA
1. Set your base DCA amount (e.g., $1,000)
2. This is the amount before multiplier adjustment

### Step 2: Enable Enhanced DCA
1. Check the "Enable" checkbox in Enhanced Dynamic DCA section
2. Controls will become active

### Step 3: Select Strategy
Choose based on your investment philosophy:
- **RSI**: Conservative, indicator-based
- **Volatility**: Opportunistic, volatility-seeking
- **MA Crossover**: Value-oriented
- **Momentum**: Contrarian, buy-the-dip

### Step 4: Set Multiplier Range
- **Min Multiplier** (default 0.5): Minimum investment during unfavorable conditions
- **Max Multiplier** (default 2.0): Maximum investment during favorable conditions

Example:
- Base DCA: $1,000
- Min: 0.5 → Minimum $500
- Max: 2.0 → Maximum $2,000

### Step 5: Configure Lookback Period
- Default: 14 days
- Shorter (7-10): More responsive to recent changes
- Longer (20-30): Smoother, less reactive

### Step 6: Run Simulation
- Click "Run" to backtest with Enhanced DCA
- Review transaction logs to see multiplier adjustments
- Compare with standard DCA results

---

## Configuration Examples

### Conservative Profile
```
Strategy: RSI-Based
Min Multiplier: 0.7
Max Multiplier: 1.5
Lookback: 14 days
```
**Behavior:** Moderate adjustments, buy more on clear oversold signals

---

### Aggressive Profile
```
Strategy: Volatility-Based
Min Multiplier: 0.3
Max Multiplier: 3.0
Lookback: 7 days
```
**Behavior:** Large adjustments, heavily buy into volatility

---

### Value Investor Profile
```
Strategy: MA Crossover
Min Multiplier: 0.5
Max Multiplier: 2.0
Lookback: 20 days
```
**Behavior:** Buy more when price is below average, patient approach

---

### Contrarian Profile
```
Strategy: Momentum (Contrarian)
Min Multiplier: 0.4
Max Multiplier: 2.5
Lookback: 10 days
```
**Behavior:** Maximum buying during downtrends, minimal during uptrends

---

## Benefits

### 1. **Automated Timing**
- No manual decision-making required
- Systematic approach removes emotions
- Consistent application of strategy

### 2. **Cost Averaging Optimization**
- Buy more when assets are cheaper
- Reduce exposure when assets are expensive
- Improves overall cost basis

### 3. **Flexibility**
- Four distinct strategies for different market views
- Adjustable parameters for personalization
- Compatible with all portfolio strategies

### 4. **Risk Management**
- Min/Max bounds prevent extreme positions
- Configurable lookback periods
- Transparent multiplier calculations

### 5. **Performance Tracking**
- Detailed transaction logs
- Visual indicators in results
- Easy comparison with standard DCA

---

## Technical Details

### RSI Calculation
```
RSI = 100 - (100 / (1 + RS))
RS = Average Gain / Average Loss
```

### Volatility Calculation
```
σ = √(Σ(r - r̄)² / N)
```

### Moving Average
```
MA = Σ(Price) / N
Price-to-MA = Current Price / MA
```

### Momentum
```
Momentum = (Current Price - Past Price) / Past Price
```

---

## Testing Recommendations

### 1. Baseline Test
- Run simulation with standard DCA first
- Note: CAGR, Sharpe, MDD

### 2. Strategy Comparison
- Test each strategy with same parameters
- Compare results across different market conditions

### 3. Parameter Sensitivity
- Test different multiplier ranges (0.3-3.0 vs 0.7-1.3)
- Test different lookback periods (7, 14, 21 days)

### 4. Market Conditions
- Bull market performance
- Bear market performance
- Volatile/choppy market performance

---

## Version Information
- **Feature Version:** v48.3
- **Added:** March 31, 2026
- **Compatible with:** All existing strategies (Equal, Rank, Top3, AAA, etc.)
- **Market Support:** ETF & Crypto

---

## Future Enhancements (Roadmap)
- [ ] Combination strategies (RSI + Volatility)
- [ ] Machine learning-based multiplier
- [ ] Custom indicator support
- [ ] Historical multiplier visualization chart
- [ ] DCA statistics dashboard
- [ ] Export DCA decision log

---

## Support & Documentation
For detailed mathematical formulas and implementation details, see:
- [README.md](README.md) - Section 7: Enhanced Dynamic DCA
- Transaction logs in simulation results
- Inline help tooltips (hover over ⓘ icons)
