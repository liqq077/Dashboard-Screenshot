# 📈 AlphaQuant Pro - A-Share Quantitative Backtesting Platform

![React](https://img.shields.io/badge/React-18.0-blue?style=flat-square&logo=react)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-3.0-38bdf8?style=flat-square&logo=tailwindcss)
![Recharts](https://img.shields.io/badge/Visualization-Recharts-orange?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

Website:
https://stock-opal-seven.vercel.app/

**AlphaQuant Pro** is a modern, interactive backtesting engine for the A-Share market, built with React. 

It provides a professional dashboard to visualize historical data, simulate trading strategies (MACD & Bollinger Bands), and analyze performance metrics. The platform features a unique data handling system that supports direct API connections, deterministic simulations, and a local Python bridge to bypass browser CORS restrictions.

## ✨ Key Features

* **📊 Interactive Visualization**
  
  High-performance composed charts powered by `Recharts`, overlaying candlestick data with technical indicators (Upper/Lower Bands, Moving Averages). Equipped with a menu collapse button to make the interface layout more concise and intuitive.

* **🌐 Bilingual Mode**

  Supports easy switching between Chinese and English, which can meet the usage needs of users with different language backgrounds.

* **📈 Underlying Asset & Time Section**

  Stock codes can be entered in the "Custom Code" input box, and this function supports querying and operating all A-share underlying assets.

* **💰 Trading Execution Plan Section**
    * **Position and Principal Allocation**
  
      It supports customizing the position ratio for a single stock, while allowing flexible setting of the initial principal. For example, if the initial principal is set to 500,000 yuan, you can specify to use only 20% of it (i.e., 100,000 yuan) to buy Kweichow Moutai, and the remaining funds will not participate in the trading of this stock, thus realizing flexible fund allocation.
  
    * **Stop-Loss, Take-Profit, and Backtest Triggering**
  
      It supports customizing stop-loss/take-profit ratios. When the preset ratio is triggered, a mandatory position-closing operation will be executed automatically (stop-loss corresponds to mandatory sell-to-limit-loss, and take-profit corresponds to mandatory sell-to-lock-in-profit). After all parameters are configured, click "Run Backtest" to start strategy verification and test the performance of the plan in historical market conditions.
  
    * **Market Trend Filter and Chart Function**
       * **Core Logic of the Filter** ——— The Market Trend Filter supports enabling/disabling, and a built-in detailed plan manual is provided to assist understanding.
  
         * **When enabled**: 
  
           The 60-day Moving Average (MA60) is used as the trend judgment standard — when the stock price is above the 60-day MA, the trend is determined to be positive, and buying is allowed; when the stock price is below the 60-day MA, it is determined to be a weak market (bear market), and buying is prohibited to avoid counter-trend operations.
  
         * **When disabled**: 
  
           It is not restricted by the 60-day Moving Average (MA60), and trades are executed only in accordance with the preset buying and selling rules. 
       * **Chart Visualization Design**
         * **Trend and Trading Volume Display**
  
           When the filter is enabled, a purple 60-day Moving Average (MA60) line will be added to the chart. Meanwhile, trading volume data is displayed synchronously with a dual Y-axis design. Since the unit magnitudes of price and trading volume differ significantly, the trading volume Y-axis is hidden to keep the chart concise. Hover the mouse over a specific date to view the detailed trading volume of that day, which helps determine the price-volume matching degree. 
  
         * **Accurate Market Data Viewing**
  
           A scrollbar is provided below the chart, supporting custom selection of market data for specific time periods. It allows zooming in on local intervals to improve the accuracy of market observation.

* **🧠 Core Strategy Model Section – Dual-Strategy Engine**
    * **Bollinger Bands**

      Mean reversion strategy (Buy on Lower Band touch, Sell on Upper Band touch).
  
    * **MACD**
  
      Trend following strategy (Buy on Golden Cross, Sell on Death Cross). Note: When encountering specific underlying assets (such as a particular stock or index), only minor parameter adjustments are required.
  
   * **Note 📒**
   
     When encountering specific underlying assets (such as a particular stock or index), only minor parameter adjustments are required.
   * **Practical Application Scenario Comparison Table for Bollinger Bands & MACD Strategies** ⬇️
    
     | Market Type | Adapted Strategy | Core Parameters (Classic Values + Adjustment Tips) | Practical Steps (3 Steps to Complete) | Stop Loss / Filter Rules (Avoid Pitfalls) |
     | :--- | :--- | :--- | :--- | :--- |
     | **Oscillating Market (price consolidates back and forth, no clear direction)** | `Bollinger Bands (mean reversion)` | Middle Band: 20-day SMA; Upper/Lower Bands: 2x standard deviation→ If volatility is intense → adjust standard deviation to 2.5x; if volatility is mild → 1.5x | 1. Observe price fluctuations between "Middle Band ± Bands";2. Price touches lower band + trading volume expands → Buy;3. Price touches upper band + trading volume shrinks → Sell | Stop Loss: If the price drops more than 1% below the lower band after buying (e.g., lower band at 10 yuan, drops below 9.9 yuan) → Stop loss immediately;Filter: Only act on signals of "pullback after touching the bands" (to avoid false breakouts) |
     | **Unilateral Uptrend Market (price continues to rise, few pullbacks)** | `MACD (trend following)` | Fast EMA: 12-day; Slow EMA: 26-day; Signal Line: 9-day→ If trend is strong → adjust fast EMA to 10-day; if trend is weak → 15-day | 1. Wait for MACD to form a "Golden Cross" (DIF crosses above DEA) + red histogram appears;2. Red histogram continues to expand after the Golden Cross → Buy and hold positions;3. MACD forms a "Death Cross" (DIF crosses below DEA) → Sell and exit | Stop Loss: If the price drops below the recent low after buying (e.g., recent low at 12 yuan when buying, drops below 12 yuan) → Stop loss;Filter: Only act on Golden Crosses above the zero line (to avoid weak rebound signals) |
     | **Unilateral Downtrend Market (price continues to decline, few rebounds)** | `MACD (trend following)` | Same as parameters for unilateral uptrend (no adjustment needed) | 1. Wait for MACD to form a "Death Cross" (DIF crosses below DEA) + green histogram appears;2. Green histogram continues to expand after the Death Cross → Go short (or sell holdings);3. MACD forms a "Golden Cross" → Stop shorting | Stop Loss: If the price breaks above the recent high after going short (e.g., recent high at 8 yuan when shorting, breaks above 8 yuan) → Stop loss;Filter: Only act on Death Crosses below the zero line (to avoid weak pullback signals) |
     | **Oscillating-to-Unilateral Market (price breaks out of the consolidation range)** | `Bollinger Bands first, then MACD` | Bollinger Bands: 20-day SMA + 2x standard deviation; MACD: classic parameters | 1. Bollinger Bands width narrows (late stage of oscillation) → Focus closely;2. Price breaks above upper band + MACD Golden Cross → Buy;3. MACD Death Cross → Sell | Stop Loss: If the price pulls back and breaks below the upper band (original resistance turns to support) after the breakout → Stop loss;Filter: Trading volume must expand by more than 30% at the time of breakout (to confirm a valid breakout) |

  * **Quick Tips for Judging Market Trends**
  
    1️⃣ Check the relationship between price and the middle band:Price repeatedly crosses the middle band → oscillating market (use Bollinger Bands);Price stays consistently above/below the middle band → trending market (use MACD);

    2️⃣ Check the width of Bollinger Bands:Band width continues to expand → trend strengthens (switch to MACD);Band width continues to narrow → oscillations intensify (use Bollinger Bands);

    3️⃣ Check the MACD histogram:Red histogram/green histogram continues to lengthen → clear trend (use MACD);Frequent switching between red and green histograms → oscillating market (use Bollinger Bands).

* **🛠 Dynamic Parameter Tuning**
  
  Adjust strategy parameters (Window, StdDev, Fast/Slow periods) in real-time and instantly observe changes in the equity curve.

* **💾 Flexible Data Sources**
    * **Mock Mode**: Deterministic, seed-based simulation for instant demos without API keys.
  
    * **Python Bridge**: Built-in generator for **Baostock/Tushare** scripts to fetch real data locally and import via JSON (solves browser CORS issues).
  
    * **Tushare API**: Direct integration for users with valid tokens and proxy configurations.
  
    * **Obtained via Sina API Interface**

* **📉 Comprehensive Metrics**
  
  * Automatic calculation of **Total Return**, **Max Drawdown**, **Win Rate**, **Total Trades**, and **Final Equity**.
  * **Note 📒**
  
    The total return rate has deducted handling fees (the deduction item is not displayed in the chart, but handling fees have been incorporated into the calculation logic).


* **🎨 Professional UI** 

  Fully responsive Dark Mode design using Tailwind CSS with Lucide icons.

## 🚀 Tech Stack

* **Framework**: [React](https://reactjs.org/) (Vite/CRA)

* **Styling**: [Tailwind CSS](https://tailwindcss.com/)

* **Charts**: [Recharts](https://recharts.org/)

* **Icons**: [Lucide React](https://lucide.dev/)

* **Data Logic**: Pure JavaScript implementation of financial algorithms.

## 📦 Installation & Setup

1.  **Clone the repository**
    ```bash
    git clone [https://github.com/your-username/alpha-quant-pro.git](https://github.com/your-username/alpha-quant-pro.git)
    cd alpha-quant-pro
    ```

2.  **Install dependencies**
    ```bash
    npm install
    # or
    yarn install
    ```

3.  **Start the local server**
    ```bash
    # Start the front-end service (responsible for interface display)
    npm start
    # or
    yarn dev
    # Start the newly added back-end service (responsible for data processing and interface support)
    node server.js
    ```

4.  **Access the app**
    Open `http://localhost:5173` in your browser.

* **Notes 📒** (For Developers/Troubleshooting)

  * **Front-end service** runs on http://localhost:5173 (user-facing interface, no need to change);

  * **Back-end API service** runs on http://localhost:3001 (automatically called by front-end, no need to access directly);

  * Ensure both services are running at the same time (use separate terminal windows).


## 📖 Usage Guide

### 1. Data Acquisition (The "Python Bridge")
**1️⃣ Method 1: Enter the stock code directly or select one of the 4 stocks (Highly Recommended) ⬇️**

Enter the stock code in the "Custom Code" input box. This function is compatible with all A-share underlying assets in the market, featuring convenient operation and an intuitive interface.

**2️⃣ Method 2: "Python Bridge" Function ⬇️**

Since web browsers restrict direct cross-origin requests (CORS) to some financial APIs (like Baostock), this platform includes a helper tool:

* Select **"Baostock"** or **"Tushare"** in the settings panel;

* Click **"Get Data Script"**;

* The app generates a Python snippet based on your selected stock and date range;

* Run the script locally:
    ```bash
    # Ensure you have dependencies installed
    pip install baostock pandas tushare
    python fetch_data.py
    ```

* Copy the JSON output from your terminal and paste it into the **"Manual Import"** box in the web app.

### 2. Strategy Configuration
You can toggle between strategies and adjust their sensitivity:

| Strategy | Parameter | Description |
| :--- | :--- | :--- |
| **Bollinger** | `Period` | The moving average window (N days). |
| **Bollinger** | `Multiplier` | Standard deviation multiplier (Width of bands). |
| **MACD** | `Fast (Short)` | Fast EMA period (Default: 12). |
| **MACD** | `Slow (Long)` | Slow EMA period (Default: 26). |
| **MACD** | `Signal` | DEA Signal line period (Default: 9). |

### 3. Analyzing Results
* **Chart Tab**: View Buy/Sell signals directly on the K-line chart.

* **Equity Tab**: Track the growth of your capital over time.

* **Trades Tab**: detailed log of every transaction, including execution price and reasoning.

## 📸 Screenshots
**Dashboard Screenshots**

Below are the bilingual interface screenshots of AlphaQuant Pro (taking Kweichow Moutai's backtest as an example), showing core functions like data source configuration, backtest results, and equity curves.

**1. English Version**
<div align="center">
  <img src="https://github.com/liqq077/Dashboard-Screenshot/blob/main/images/Dashboard-English.png?raw=true" 
       alt="AlphaQuant Pro Dashboard (English) - Moutai Backtest" 
       width="800"/>
  <p style="color:#666; margin-top:8px;">Key Display: Total Return (-6.63%), Final Equity (¥46.7w), Bollinger Bands Strategy Settings</p>
</div>

**2. Chinese Version**
<div align="center">
  <img src="https://github.com/liqq077/Dashboard-Screenshot/blob/main/images/Dashboard-Chinese.png?raw=true" 
       alt="AlphaQuant Pro Dashboard (中文) - 贵州茅台回测" 
       width="800"/>
  <p style="color:#666; margin-top:8px;">核心展示：总收益率(-6.63%)、最终资金(¥46.7w)、布林带策略参数配置</p>
</div>



</div>

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1.  Fork the Project

2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)

3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)

4.  Push to the Branch (`git push origin feature/AmazingFeature`)

5.  Open a Pull Request

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

<div align="center">
  <p>Built with ❤️ 
</div>
