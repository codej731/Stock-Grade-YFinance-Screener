# 📈 North American Equity Research & Screener

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Finance](https://img.shields.io/badge/Finance-Quantitative-green)
![Market](https://img.shields.io/badge/Markets-US%20%26%20Canada-red)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

A comprehensive Python-based equity research engine designed to filter, analyze, and grade stocks across the **US (NASDAQ/NYSE)** and **Canadian (TSX/TSX-V)** markets. This tool automates fundamental analysis by applying credit risk models, valuation screens (Buffett/Graham), and institutional sentiment filters to identify high-quality investment candidates.

## 📋 Overview

This project pulls raw market data, filters out uninvestable sectors (or targets them specifically), and applies a custom **Credit Strength Model** to categorize stocks into actionable "buckets."

**Key Capabilities:**
* **Dual-Market Support:** Automatically handles currency and reporting differences between US and Canadian exchanges.
* **Credit Bucketing:** Grades companies into **Fortress** (Safe), **Moonshot** (Turnaround), or **Distress** (Avoid).
* **Valuation Logic:** Implements "Buffett NAV" screens (Price < Book) and Deep Value metrics.
* **Analyst Alignment:** Cross-references technical screens with Analyst Consensus (Buy/Strong Buy only).
* **Options Integration:** Checks for option chain liquidity (US markets) to support hedging strategies.

## 🧠 The Grading Logic

The core of this project is the **Credit Model**, which sorts non-financial companies into three distinct categories based on their Z-Score, ROIC, and Interest Coverage.

| Bucket | Profile | Criteria Highlights |
| :--- | :--- | :--- |
| 🛡️ **Fortress** | **High Quality / Safe** | Z-Score > 2.9, ROIC > 5%, Interest Coverage > 4x. Proven stability. |
| 🚀 **Moonshot** | **Turnaround / Growth** | Z-Score < 2.5 but **Margin Trend is Improving**. High risk, high reward potential. |
| ⚠️ **Distress** | **Bankruptcy Risk** | Z-Score < 1.8 or Interest Coverage < 1.5. Statistically likely to fail. |

## 📂 Repository Structure

The project is split into focused notebooks for different sectors and regions:

### 🇺🇸 United States 
* `Equity_Research_USA_Analyst_Rating.ipynb`
    * **The Master Screener.** Scrapes US tickers, filters by "Buy/Strong Buy" analyst ratings, runs the Credit Model, and checks for Options liquidity. This is the primary tool for finding quality plays.
    * *Includes:* A "Watchlist Combiner" that merges ticker data with Yahoo Finance stats.

### 🇨🇦 Canadian Specific
* `Equity_Research_CA_Nonfinancial_Buffet.ipynb`
    * Focuses on TSX/TSX-V stocks. Combines the Credit Model with a **Warren Buffett NAV filter** (Price < Book + Profitable) to find "Net-Net" style deep value.
* `Equity_Research_CA_Nonfinancial.ipynb`
    * The standard credit analysis for Canadian operating companies (Mining, Energy, Tech, Retail), excluding banks.
* `Equity_Research_Canadian_Financials.ipynb`
    * Specialized screener for Canadian Banks, Insurance, and REITs (since standard Z-Scores don't apply to financials).

### 🏦 Financial Sector (Specialized)
* `Equity_Research_USA_Financials.ipynb`
    * Dedicated screener for US Financials. Outputs three specific lists: **Undervalued** (Low P/B), **Income** (High Yield), and **Growth** (High P/E + Strong Buy).

## 🛠️ Installation & Usage

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/codej731/Stock-Grade-YFinance-Screener.git](https://github.com/codej731/Stock-Grade-YFinance-Screener.git)
    cd Stock-Grade-YFinance-Screener
    ```

2.  **Install Dependencies:**
    ```bash
    pip install pandas yfinance numpy requests yahooquery openpyxl scipy finvizfinance
    ```
    *Note: `yahooquery` is essential for the fast, asynchronous data fetching used in the screeners.*

3.  **Run a Screener:**
    Open any notebook (e.g., `Equity_Research_USA_Analyst_Rating.ipynb`) in Jupyter or VS Code.
    * **Step 1:** The script fetches the ticker universe (thousands of stocks).
    * **Step 2:** It filters for liquidity (> $300M Cap, High Volume) and Sector.
    * **Step 3:** It performs the Deep Dive analysis (Math calculations).
    * **Step 4:** It saves the results to an Excel file in your specified directory (default is OneDrive, check the `save_to_excel` function to update the path).

## 📊 Example Output

The tools export Excel files with tabbed sheets for easy sorting:

* **Sheet 1: Fortress** (Sorted by Z-Score Safety)
* **Sheet 2: Moonshot** (Sorted by Growth Potential)
* **Sheet 3: Distress** (Sorted by Bankruptcy Risk)

## ⚠️ Disclaimer

This software is for **educational and research purposes only**. The analysis is based on historical data from third-party sources (Yahoo Finance) which may be delayed or erroneous. **Do not make investment decisions based solely on this code.**
