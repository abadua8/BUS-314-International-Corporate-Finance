# Caterpillar Inc. (CAT) — Accounting & Performance Ratios
## Technical Specification | Stage 3

| | |
|---|---|
| **Created by:** | Allan Badua |
| **Updated by:** | Allan Badua |
| **Date Created:** | April 2025 |
| **Date Updated:** | April 2025 |
| **Version:** | 1.0 |
| **LLM Used:** | Claude (Anthropic) — organizational support |
| **Role:** | Financial Analyst / FP&A Analyst |
| **Audience:** | CFO or Director of FP&A |

---

## 1. Problem Statement

Caterpillar Inc. (CAT) is a publicly traded Industrials company specializing in heavy machinery and equipment. This specification outlines the analytical framework for computing 25+ accounting and performance ratios from the company's FY2024 financial statements (fiscal year ending December 31, 2024), enabling management to assess financial health, operational efficiency, leverage, and value creation.

| Dimension | Detail |
|---|---|
| **Company** | Caterpillar Inc. (NYSE: CAT) |
| **Industry** | Industrials — Heavy Machinery & Equipment |
| **Fiscal Years** | FY2024 (current) and FY2023 (prior year, used as start-of-year denominators) |
| **Objective** | Compute and interpret 25+ financial ratios across performance, profitability, efficiency, leverage, liquidity, and Du Pont decomposition categories |
| **Decision Context** | CFO briefing and board-level financial health review |
| **Data Source** | Caterpillar 10-K FY2024, SEC EDGAR |

---

## 2. Inputs (Known Variables)

All figures are in millions of U.S. dollars. Inputs are organized by financial statement source and assigned standardized named ranges for model auditability.

### Balance Sheet Items

| Variable | Named Range | FY2024 | FY2023 | Year(s) |
|---|---|---:|---:|---|
| Cash & cash equivalents | `BAL_cash_marketable_securities_[year]` | 6,889 | 6,978 | Both |
| Receivables (trade & other) | `BAL_receivables_[year]` | 9,282 | 9,310 | Both |
| Inventories | `BAL_inventories_[year]` | 16,827 | 16,565 | Both |
| Total current assets | `BAL_assets_current_[year]` | 45,682 | 46,949 | Both |
| Net tangible fixed assets | `BAL_fixed_assets_net_[year]` | 13,361 | 12,680 | Current |
| Total assets | `BAL_assets_total_[year]` | 87,764 | 87,476 | Both |
| Total current liabilities | `BAL_liabilities_current_[year]` | 32,272 | 34,728 | Current |
| Long-term debt (due after 1 yr) | `BAL_debt_long_term_[year]` | 27,351 | 24,472 | Both |
| Total liabilities | `BAL_liabilities_total_[year]` | 68,270 | 67,973 | Current |
| Shareholders' equity | `BAL_equity_shareholders_[year]` | 19,494 | 19,503 | Both |

### Income Statement & Cash Flow Items

| Variable | Named Range | FY2024 | Source |
|---|---|---:|---|
| Net sales | `INC_sales` | 64,809 | Income Stmt |
| Cost of goods sold | `INC_cost_goods_sold` | 43,244 | Income Stmt |
| SG&A expenses | `INC_sga` | 5,965 | Income Stmt |
| Depreciation | `INC_depreciation` | 1,413 | Income Stmt |
| EBIT | `INC_ebit` | 14,187 | Income Stmt |
| Other income | `INC_other_income` | 2,116 | Income Stmt |
| Interest expense | `INC_interest_expense` | 1,713 | Income Stmt |
| Taxes | `INC_taxes` | 2,944 | Income Stmt |
| Net income | `INC_net` | 11,646 | Income Stmt |
| Dividends | `INC_dividends` | 2,736 | Income Stmt |
| Cash from operations | `CASH_operating` | 14,184 | Cash Flow |
| Cash from investments | `CASH_investments` | (2,186) | Cash Flow |

### Market / Analyst Inputs

| Variable | Named Range | Value | Notes |
|---|---|---:|---|
| Share price | `share_price` | $349.58 | End of FY2024 closing price |
| Shares outstanding | `shares_outstanding` | 492M | In millions |
| Cost of capital (WACC) | `cost_capital` | 7.9% | Analyst estimate |
| Tax rate | `tax_rate` | 21.4% | Effective rate from 10-K |

---

## 3. Assumptions & Constraints

- All figures reported in millions of U.S. dollars unless otherwise noted.
- Tax rate set at 21.4% (effective rate from CAT's FY2024 financial statements).
- Cost of capital estimated at 7.9% WACC based on analyst convention; students may refine using CAPM methodology.
- Share price of $349.58 reflects the end-of-fiscal-year closing market price.
- Depreciation is drawn from the Income Statement, not the Cash Flow Statement.
- Start-of-year values use the FY2023 Balance Sheet as the denominator for ROA, ROC, ROE, and turnover ratios.
- Long-term debt is limited to obligations due after one year ($27,351M); the current portion of long-term debt is excluded from leverage naming to match template conventions.
- No off-balance-sheet items, contingent liabilities, or segment-level data are incorporated.
- After-tax operating income is computed as: Net Income + (1 − Tax Rate) × Interest Expense, preserving operating return independent of financing decisions.

---

## 4. Calculation Flow

### Step 1: Derived Inputs

| Derived Variable | Formula (Named Range Pseudocode) |
|---|---|
| `market_capitalization` | `share_price` × `shares_outstanding` = $171,993M |
| `currentYear_after_tax_operating_income` | `INC_net` + (1 − `tax_rate`) × `INC_interest_expense` = $11,646M* |
| `currentYear_daily_sales_average` | `INC_sales` / 365 = $177.56M/day |
| `currentYear_working_capital_net` | `BAL_assets_current_2024` − `BAL_liabilities_current_2024` = $13,410M |
| `avg_equity` | AVERAGE(`startYear_equity`, `currentYear_equity`) = $19,499M |
| `startYear_total_capitalization` | `BAL_debt_long_term_2023` + `BAL_equity_shareholders_2023` = $43,975M |

*Note: The model's after-tax operating income ($11,646M) effectively equals Net Income at CAT's debt/income ratio. This warrants review in the next iteration.*

### Steps 2–7: Ratio Computation Summary

| Category | Key Ratios | Key Results |
|---|---|---|
| **Performance** | MVA, Market-to-Book, EVA | MVA = $152,499M; EVA = $15,100M |
| **Profitability** | ROA, ROC, ROE (start-year & avg denominators) | ROA = 13.3%; ROE = 59.7% on book equity |
| **Efficiency** | Asset turnover, receivables turnover, inventory turnover, collection period, days in inventory, profit margin | Inventory days = 140; Avg collection = 52 days |
| **Leverage** | LT debt ratio, debt-equity, total debt ratio, TIE, cash coverage, debt burden, leverage ratio | TIE = 8.3x; Cash coverage = 9.1x |
| **Liquidity** | NWC-to-Assets, current ratio, quick ratio, cash ratio | Current ratio = 1.42x (corrected); Cash ratio = 0.21x |
| **Du Pont** | ROA = Asset Turnover × Operating Profit Margin; ROE = Leverage × Turnover × Margin × Debt Burden | Du Pont ROA = 13.3%; Du Pont ROE = needs correction |

---

## 5. Outputs

| Output | Description | Format | Purpose |
|---|---|---|---|
| Ratio summary table | 25+ ratios organized across 6 categories | Table | Core analytical output |
| Du Pont decomposition | ROA and ROE broken into component drivers | Table | Return driver analysis |
| Named range index | Formula pseudocode for every ratio in column D | Column | Auditability |
| Color-coded model | Blue = inputs, yellow = data, light yellow = output cells | Formatting | Model clarity |
| Executive summary (Stage 4) | Key findings and strategic recommendations for CFO | Memo | Stage 4 input |

---

## 6. Model Review — What Worked & What to Improve

### What Worked Well

- The named range architecture (e.g., `BAL_equity_shareholders_2024`, `INC_ebit`) made formulas self-documenting and easy to audit across the Ratios tab without cell-reference hunting.
- The Du Pont decomposition matched the direct ROA calculation exactly (13.3%), confirming internal consistency for the asset return driver.
- Color coding (blue inputs, yellow data, light yellow output cells) clearly delineated analyst-controlled assumptions from pulled financial data.
- The balance sheet balanced to zero for both FY2024 and FY2023, confirming data entry accuracy from the 10-K source.
- The after-tax operating income formula structure (Net Income + interest tax shield) was correctly applied and referenced throughout all profitability ratios.

### What to Improve

- `BAL_liabilities_current_2024` was incorrectly mapped to Customer Advances ($2,322M) rather than Total Current Liabilities ($32,272M). This distorted the current ratio, quick ratio, cash ratio, NWC, and related leverage ratios and must be corrected before Stage 4 interpretation.
- `BAL_debt_long_term_2024` was similarly mapped to Other Current Liabilities ($2,909M) instead of Long-Term Debt Due After One Year ($27,351M), corrupting leverage and total capitalization calculations.
- `BAL_liabilities_total_2024` returned 0 — the named range was not assigned or pointed to an empty cell, making the total debt ratio unusable.
- ROC and ROE using start-of-year equity show negative values because prior-year Treasury Stock caused a structurally negative reported equity position. A footnote or alternative equity definition would improve interpretability for executive audiences.
- The Du Pont ROE diverges from direct ROE due to the corrupted equity and capitalization named ranges. Once cell references are corrected, these should reconcile.

### What Would Make the Model More Auditable

- Add an explicit Input Check tab that cross-references every named range against the source financial statement cell, flagging any mismatches.
- Extend the formula documentation column (Column D) to all ratio rows — currently only partially complete.
- Use conditional formatting to flag ratios outside expected industry ranges (e.g., current ratio below 1.0x, TIE below 2.0x) for instant anomaly detection.

### Additional Analysis Worth Including

- Year-over-year trend comparison (FY2022–FY2024) to contextualize FY2024 ratio changes.
- Industry peer benchmarking against Deere & Company (DE) and CNH Industrial for competitive positioning.
- Free cash flow yield and capex intensity ratios, given CAT's significant capital expenditure profile ($1,702M in FY2024).

---

## 7. Limitations & Next Steps

This specification does not incorporate industry peer comparisons, multi-year trend analysis, segment-level breakdowns, or off-balance-sheet obligations (e.g., operating lease commitments). The negative equity values driven by Caterpillar's aggressive share repurchase program ($7,839M in FY2024 alone) create structural distortions in equity-based ratios that require interpretive context rather than face-value readings.

The next phase (Stage 4) will involve writing a structured AI prompt to re-run the corrected model and producing a final executive memo interpreting ratio results for CFO-level decision-making, including commentary on CAT's leverage, return profile, and capital allocation strategy.
