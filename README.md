# GAS Price Levels Technical Indicator

## Overview
The GAS Price Levels indicator is a sophisticated multi-factor technical analysis tool that combines traditional technical indicators with advanced alpha factors to provide comprehensive price level analysis for natural gas trading. This indicator implements a dynamic weighting system based on Information Coefficients (IC) to adapt to changing market conditions.

## Technical Components

### Core Technical Indicators
1. **Bollinger Bands (BB)**
   - Length: 20 periods (customizable)
   - Standard Deviation: 2.0 (customizable)
   - Implementation: `ta.bb(close, bbandsLength, bbandsStdDev)`
   - Provides dynamic support/resistance levels based on volatility

2. **Kaufman's Adaptive Moving Average (KAMA)**
   - Length: 10 periods
   - Implementation: `ta.ema(close, 10)`
   - Adapts to market volatility for more responsive trend following

3. **Additional Supporting Indicators**
   - RSI (Relative Strength Index): 14-period default
   - CCI (Commodity Channel Index): 20-period default
   - ATR (Average True Range): 10-period default
   - Williams %R: 14-period default
   - MACD: (12, 26, 9) standard parameters

### Advanced Alpha Factors

#### Alpha238 (40-day Minimum LogReturn)
```pine
cumLogReturn40d = ta.cum(math.log(close / close[1]))
alpha238 = ta.lowestbars(cumLogReturn40d, 40)
```
Mathematical foundation:
- Calculates cumulative log returns over 40 days
- Identifies historical price patterns through minimum return periods
- Effective for mean reversion strategies

#### Alpha51 (Price ROC Interaction)
```pine
alpha51 = ta.roc(close, 30) * ta.roc(close, 5)
```
Properties:
- Combines long-term (30-period) and short-term (5-period) price momentum
- Identifies momentum divergences and convergences
- Multiplication effect amplifies significant price moves

#### Alpha262 (CCI Complex)
```pine
alpha262 = ta.change(ta.sma(math.pow(cci, 2), cciLength))
```
Characteristics:
- Squares CCI values to emphasize extreme movements
- Applies smoothing through SMA
- Tracks rate of change in volatility

### Dynamic Weighting System

The indicator implements a sophisticated dynamic weighting system based on Information Coefficients (IC):

```pine
float ic_238 = ta.correlation(alpha238, ta.change(close), 20)
float ic_51 = ta.correlation(alpha51, ta.change(close), 20)
float ic_262 = ta.correlation(alpha262, ta.change(close), 20)
float total_ic = math.abs(ic_238) + math.abs(ic_51) + math.abs(ic_262)
float w238 = math.abs(ic_238) / total_ic
```

#### Weight Calculation Method
1. Calculates 20-period correlation between each alpha and price changes
2. Takes absolute values to focus on strength rather than direction
3. Normalizes weights by dividing by total IC
4. Automatically adjusts factor importance based on recent predictive power

## Usage Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| emaPeriods | 9 | EMA calculation length |
| atrLength | 10 | ATR calculation period |
| bbandsLength | 20 | Bollinger Bands period |
| bbandsStdDev | 2.0 | BB standard deviation multiplier |
| rsiLength | 14 | RSI calculation period |
| cciLength | 20 | CCI calculation period |

## Visual Components

The indicator displays several overlay elements on the price chart:
- Bollinger Bands (Upper, Middle, Lower) in blue/white with 80% transparency
- KAMA line in orange with 80% transparency

## Implementation Notes

1. **Performance Considerations**
   - Uses efficient `ta.*` built-in functions for optimal performance
   - Implements look-back periods carefully to minimize repainting
   - Calculations are streamlined to reduce computational overhead

2. **Risk Management**
   - Multiple timeframe analysis recommended (indicator works on any timeframe)
   - Use in conjunction with position sizing based on ATR values
   - Consider BB width for volatility-based risk adjustment

## Requirements
- TradingView Pine Script version 5
- Real-time or delayed data feed for natural gas prices

## Installation
1. Open TradingView Chart
2. Go to Pine Editor
3. Create New Indicator
4. Copy and paste the provided code
5. Add to Chart

## License
MIT License

## Author
Denizhan Dakılır

## Version History
- v1.0.0: Initial release
- [Additional versions as needed]
