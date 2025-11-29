# HP Bubble Dual MTF Indicator v1.03

## Overview

HP Bubble with Dual Multi-Timeframe Display is a technical indicator for MetaTrader 5 that shows both current timeframe and higher timeframe analysis using the Hodrick-Prescott Filter with automatic λ scaling.

Version: 1.03  
Developer: code_finance  
Contact: https://t.me/code_finance  
Copyright: 2025, Optimized HP Filter Indicator  
Expiration Date: 2025.12.17

---

## Features

- Dual Multi-Timeframe Analysis: Displays current timeframe + automatically selected higher timeframe
- Hodrick-Prescott Filter: Advanced statistical method for trend-cycle decomposition
- Automatic Lambda Scaling: Adapts smoothing parameter based on timeframe
- Visual Status Label: Shows higher timeframe market status in real-time
- Color-Coded Signal Line: 5 color zones indicating market condition
- Bubble Histogram: Displays deviation from trend as percentage

---

## Indicator Components

### 1. Bubble Histogram
- Positive Bubble (Green): Price above trend
- Negative Bubble (Red): Price below trend
- Shows percentage deviation from HP Filter trend

### 2. Signal Line
Color-coded moving average with 5 market states:
- 🔴 Red - Strong Overbought (>15%)
- 🟠 Orange - Moderate Overbought (5-15%)
- 🟢 Green - Equilibrium (-5 to +5%)
- 🔵 Blue - Moderate Oversold (-15 to -5%)
- 🟣 Magenta - Strong Oversold (<-15%)

### 3. HTF Status Label
Real-time display showing:
- Higher timeframe name
- Current status (▲ UP / ━ NEUTRAL / ▼ DOWN)
- Bubble percentage value

---

## Input Parameters

### Multi-Timeframe Settings
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| InpHigherTimeframe | ENUM_TIMEFRAMES | PERIOD_CURRENT | Higher timeframe (CURRENT = Auto) |
| InpShowHTFStatus | bool | true | Show higher TF status label |

### HP Filter Parameters
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| InpAutoLambda | bool | true | Auto-adjust Lambda based on timeframe |
| InpManualLambda | double | 1600 | Manual Lambda (if Auto=false) |
| InpLambdaMultiplier | double | 1.0 | Lambda multiplier (fine-tune sensitivity) |
| InpWindowSize | int | 200 | Rolling window size |

### Signal Line Settings
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| InpSignalPeriod | int | 2 | Signal line period (MA) |
| InpSignalMethod | ENUM_MA_METHOD | MODE_EMA | Signal line method |

### Visual Settings
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| InpPositiveBubbleColor | color | clrDarkGreen | Positive bubble color |
| InpNegativeBubbleColor | color | clrDarkRed | Negative bubble color |

### Signal Line Colors
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| InpStrongOverbought | color | clrRed | Strong Overbought (>15%) |
| InpModerateOverbought | color | clrOrange | Moderate Overbought (5-15%) |
| InpEquilibrium | color | clrLimeGreen | Equilibrium (-5 to +5%) |
| InpModerateOversold | color | clrDodgerBlue | Moderate Oversold (-15 to -5%) |
| InpStrongOversold | color | clrMagenta | Strong Oversold (<-15%) |

### HTF Label Settings
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| InpLabelFontSize | int | 14 | Label font size |
| InpLabelXOffset | int | 10 | Label X offset from right |
| InpLabelYOffset | int | 30 | Label Y offset from top |

---

## Adaptive Lambda Values

The indicator automatically adjusts the Lambda parameter based on timeframe:

| Timeframe | Lambda | Timeframe | Lambda |
|-----------|--------|-----------|--------|
| M1 | 10 | H1 | 400 |
| M2 | 20 | H2 | 600 |
| M3 | 30 | H3 | 800 |
| M4 | 40 | H4 | 1000 |
| M5 | 50 | H6 | 1200 |
| M6 | 60 | H8 | 1600 |
| M10 | 100 | H12 | 2000 |
| M12 | 120 | D1 | 1600 |
| M15 | 150 | W1 | 10000 |
| M20 | 200 | MN1 | 40000 |
| M30 | 300 | | |

---

## Automatic Higher Timeframe Selection

When InpHigherTimeframe is set to PERIOD_CURRENT, the indicator automatically selects the next higher timeframe:

🍁𝑴𝒉𝒍𝒂🍁, [08/09/1404 09:52 ق.ظ]
| Current TF | Auto HTF |
|-----------|----------|
| M1-M4 | M5 |
| M5-M12 | M15 |
| M15-M20 | M30 |
| M30 | H1 |
| H1-H3 | H4 |
| H4-H12 | D1 |
| D1 | W1 |
| W1 | MN1 |
| MN1 | MN1 |

---

## Indicator Levels

The indicator displays reference levels at:
- 0% - Equilibrium (white solid line)
- ±5% - Moderate zones (gray dotted)
- ±10% - Moderate zones (gray dotted)
- ±15% - Strong zones (gray dotted)
- ±20% - Extreme zones (gray dotted)

---

## Calculation Methods

### HP Filter Algorithm
The indicator uses two calculation methods:

1. Thomas Algorithm (Primary)
   - Fast tridiagonal matrix solver
   - Efficient for real-time calculation
   - O(n) computational complexity

2. Iterative Method (Fallback)
   - Used if Thomas Algorithm fails
   - Maximum 100 iterations
   - Convergence tolerance: 1e-6

### Bubble Percentage Formula
Bubble% = ((Current Price - Trend) / Trend) × 100

### Signal Line Methods
Supports 4 moving average types:
- SMA - Simple Moving Average
- EMA - Exponential Moving Average (default)
- SMMA - Smoothed Moving Average
- LWMA - Linear Weighted Moving Average

---

## Usage Guidelines

### Interpreting Signals

Overbought Conditions:
- Strong Overbought (>15%): Consider taking profits or waiting for pullback
- Moderate Overbought (5-15%): Trend strength, watch for reversal signs

Equilibrium Zone (-5% to +5%):
- Price close to fair value
- Potential consolidation or trend transition

Oversold Conditions:
- Moderate Oversold (-15 to -5%): Potential buying opportunity
- Strong Oversold (<-15%): Strong reversal candidate, but confirm trend

### Multi-Timeframe Confirmation
- Bullish Setup: Both current and higher TF showing positive bubbles
- Bearish Setup: Both timeframes showing negative bubbles
- Divergence: Different signals between timeframes suggest caution

---

## Technical Requirements

- Platform: MetaTrader 5
- Minimum Data: 200 bars (window size)
- Window Type: Separate indicator window
- Buffers: 6 indicator buffers
- Plots: 3 visual plots

---

## Expiration Notice

This indicator includes an expiration mechanism:
- Expiration Date: December 17, 2025
- The indicator will stop functioning after this date
- Contact developer for renewal or updates

---

## Code Structure

### Main Functions

OnInit() - Initialization
- Validates expiration date
- Sets timeframes and lambda values
- Creates buffers and label
- Displays configuration summary

OnCalculate() - Main calculation loop
- Calculates HP Filter for current timeframe
- Updates HTF bubble value per new HTF bar
- Generates bubble histogram and signal line
- Updates status label

CalculateHPFilterThomas() - Primary HP Filter calculation
- Implements Thomas Algorithm
- Solves tridiagonal matrix system
- Returns trend and cycle components

CalculateHPFilterIterative() - Fallback calculation
- Iterative optimization method
- Used if Thomas Algorithm fails

CalculateMA() - Signal line calculation
- Supports SMA, EMA, SMMA, LWMA
- Used for smoothing bubble values

UpdateHTFLabel() - Updates status display
- Formats HTF information
- Sets appropriate colors
- Updates label text

---

## Contact & Support

Developer: code_finance  
Telegram: https://t.me/code_finance  

For:
- Technical support
- Custom modifications
- License renewal
- Bug reports

---

## Copyright & License

Copyright 2025, Optimized HP Filter Indicator  
All rights reserved.

This indicator is protected by expiration mechanism and is licensed for use until the specified expiration date.
