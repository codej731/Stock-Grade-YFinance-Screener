# Equities Fundamental Analysis Tool 📈🛡️

A powerful, algorithmic stock screener and grading tool for **US and Canadian markets**.

This project automates the process of finding high-quality, fundamentally sound companies by filtering thousands of stocks through a rigorous financial sieve. It uses a hybrid approach—leveraging `yahooquery` for speed and `yfinance` for deep financial reliability—to grade stocks into **Fortress**, **Strong**, or **Risky** tiers based on solvency, profitability, and capital efficiency.

## 🚀 Key Features

* **Multi-Market Support:** Dedicated scripts for **USA** (NYSE/NASDAQ) and **Canada** (TSX/TSX-V).
* **Hybrid Data Fetching:** Uses `yahooquery` for rapid initial screening of thousands of tickers and `yfinance` for deep-dive validation.
* **Automated Grading System:** Classifies stocks based on strict fundamental criteria (Z-Score, ROIC, Margins).
* **Specialized Scans:**
    * **🧠 Analyst Picks:** Cross-references "Fortress" stocks with Wall Street Buy ratings and upside potential.
    * **💎 Buffett Value:** Identifies deep value stocks trading below Book Value (P/B < 1) with positive ROE and manageable debt.
* **Smart Caching:** Implements a JSON caching system to prevent API throttling and speed up subsequent runs.

---

## ⚙️ How It Works

The screener operates in a **3-Step Funnel**:

### 1. The Universe Fetch
* **USA:** Pulls the latest ticker list from the NASDAQ Trader exchange data.
* **Canada:** Scrapes official TMX listings or utilizes Wikipedia constituents for TSX/TSX-V.

### 2. The Lightweight Sieve (Speed)
Filters thousands of stocks using "quick" metrics to discard junk:
* **Price:** > $2.00
* **Market Cap:** > $300M (USA) / $50M (CAD)
* **Volume:** > 1M (USA) / 100k (CAD)
* **Basic Health:** Positive Operating Margins & Current Ratio > 1.2
* **Sector Exclusion:** Removes Financials and Real Estate (as standard metrics like EBIT/Capital don't apply well).

### 3. The Deep Dive (Reliability)
Survivors undergo a rigorous "health check" using `yfinance` to calculate:
* **Altman Z-Score:** A formula used to predict bankruptcy risk.
* **ROIC (Return on Invested Capital):** Measures how efficiently a company allocates capital.
* **Interest Coverage:** Can the company pay its debts?

---

## 📊 The Grading Logic

Stocks are automatically sorted into one of three tiers based on the Deep Dive results:

| Grade | Criteria | Description |
| :--- | :--- | :--- |
| **🏰 FORTRESS** | **High Margins** (>5%)<br>**Safe Debt** (Int. Cov > 1.5)<br>**High ROIC** (>5%) | Financially bulletproof companies with efficient capital allocation and strong moats. |
| **💪 STRONG** | **Safe Debt** (Int. Cov > 1.5)<br>**High ROIC** (>5%) | Fundamentally sound companies that may have slightly lower margins but are otherwise healthy. |
| **⚠️ RISKY** | **Low Interest Coverage** (<1.5)<br>**OR Low ROIC** (<5%) | Companies that are barely profitable or carrying dangerous debt loads relative to earnings. |

---

## 🛠️ Installation & Usage

### 1. Prerequisites
You will need Python installed along with the following libraries:

```bash
pip install pandas numpy requests yahooquery yfinance finvizfinance lxml

---

### 2. Running the Screeners
There are two main notebooks depending on which market you want to analyze:

* **🇺🇸 USA Market:** Run `Equity_Res_USA_Datafolder.ipynb`
* **🇨🇦 Canadian Market:** Run `Equity_Res_CA.ipynb`

### 3. Output
The script automatically creates a folder named `YfinanceDataDump` in your directory and saves the following CSV reports:

* `fortress_stocks.csv` (The best of the best)
* `strong_stocks.csv` (Solid contenders)
* `risky_stocks.csv` (Avoid or tread carefully)
* `Analyst_Fortress_Picks.csv` (Fortress stocks that analysts also love)
* `Buffett_Value_Picks.csv` (Deep value "cigar butt" stocks)

---

## 📈 Watchlist Tool
The repo also includes a **Watchlist Combiner** (found at the end of the notebooks). This tool allows you to input a manual list of tickers (e.g., `['AAPL', 'MSFT', 'TSLA']`) and generates a consolidated view merging:
* **Technical Data:** 52-week High drop, Volatility, Relative Volume.
* **Fundamental Data:** Analyst Price Targets and Ratings (via `finvizfinance`).

---

## ⚠️ Disclaimer
*This project is for educational and research purposes only. The "Grades" (Fortress/Strong/Risky) are algorithmic outputs based on historical data and standard financial formulas. They do not constitute financial advice, and you should perform your own due diligence before investing.*
