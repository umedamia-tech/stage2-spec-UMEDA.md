## Foreign Exchange Hedging Model – Technical Specification

**Created by:** [Mia Umeda]  
**Updated by:** [Mia Umeda]  
**Date Created:** October 23, 2025  
**Date Updated:** October 23, 2025  
**Version:** 1.0  
**LLM Used:** Claude Sonnet 4

---

**Role:** Financial Analyst / Treasury Analyst  
**Audience:** CFO or Director of Treasury  
**Purpose:** Provide a professional, quantitative specification outlining the analytical structure for evaluating FX hedging alternatives for our €4.5M European receivable.

---

## 1. Problem Statement

Our company, a U.S.-based solar equipment exporter, expects to receive EUR 4,500,000 in foreign-currency revenue in 12 months from a European customer. This euro-denominated receivable exposes us to potential foreign exchange risk from adverse fluctuations in the EURUSD exchange rate during the one-year period until payment receipt. As our functional currency is USD, depreciation of the euro against the dollar would reduce our realized USD proceeds, directly impacting profit margins and cash flow predictability.

**Exposure Type:** Foreign currency receivable (long EUR position)

**Foreign Currency Amount:** EUR 4,500,000

**Time Horizon:** 1 year (365 days)

**Objective:** Protect USD value of the receivable while evaluating cost-benefit tradeoffs between certainty (forward contracts), flexibility (currency options), and operational complexity (money market hedges). The primary goal is to eliminate or significantly reduce FX volatility to ensure predictable cash flows for financial planning and budgeting purposes.

**Decision Context:** Corporate treasury function reporting to CFO, with hedge recommendations requiring executive approval before execution with banking counterparties.

This specification outlines the analytical framework for quantifying, comparing, and evaluating three alternative hedging strategies—forward contracts, currency options, and money market hedges—to mitigate foreign exchange transaction risk. The model will provide decision-support analysis to guide optimal hedge selection based on cost, certainty, and risk tolerance.

---

## 2. Inputs (Known Variables)

The following table defines all input variables required for the hedging model. These variables will serve as the foundation for spreadsheet construction and computational logic.

| Variable | Description | Unit | Example Value | Source |
|----------|-------------|------|---------------|--------|
| **FC_AMT** | Foreign-currency receivable amount | EUR | 4,500,000 | Company contract data |
| **S₀** | Current EURUSD spot rate | USD/EUR | 1.1607 | Market data (Bloomberg/Yahoo Finance) |
| **F₀** | 1-year EURUSD forward rate | USD/EUR | 1.0875 | Market data (provided) |
| **r_USD** | USD 1-year interest rate | % per annum | 3.55 | U.S. Treasury 1-year yield |
| **r_EUR** | EUR 1-year interest rate | % per annum | 2.15 | ECB deposit facility rate |
| **t** | Time to maturity | Years | 1.0 | Contract terms |
| **K_put** | EUR Put option strike price | USD/EUR | 1.1607 | Set at-the-money (ATM) |
| **K_call** | EUR Call option strike price | USD/EUR | 1.1607 | Set at-the-money (ATM) |
| **Premium_put** | Put option premium | USD per EUR | 0.015 | Market quote (scenario) |
| **Premium_call** | Call option premium | USD per EUR | 0.018 | Market quote (scenario) |

**Data Source Notes:**
- Spot and forward rates sourced from Bloomberg Terminal or equivalent financial data provider
- Interest rates reflect risk-free government securities (U.S. Treasury) and central bank policy rates (ECB)
- Option premiums represent indicative market quotes for 1-year maturity ATM options
- All rates as of market close October 23, 2025

---

## 3. Assumptions & Constraints

To ensure reproducibility and analytical clarity, the following conventions and simplifications apply:

**Rate Conventions:**
1. All interest rates are quoted on a simple annual basis (not compounded)
2. Interest rate calculations use ACT/360 day-count convention for consistency with market standards
3. Forward rate provided represents exactly 1-year maturity from spot date

**Cost Assumptions:**
4. Transaction costs (bid-ask spreads, bank fees) are excluded from base analysis
5. Counterparty credit costs and collateral requirements are not modeled
6. Option premiums are paid upfront in USD at contract inception
7. No early exercise scenarios considered for options (European-style settlement assumed)

**Exchange Rate Conventions:**
8. All exchange rates expressed as USD per EUR (American terms)
9. Spot rate at maturity (S_T) represents the realized exchange rate in one year

**Operational Assumptions:**
10. Payment occurs exactly on scheduled maturity date (no early/late payment scenarios)
11. EUR receivable amount is fixed and certain (no volume risk)
12. Company has sufficient credit facilities available for money market hedge execution
13. No tax implications or accounting hedge designation requirements included in analysis

**Exclusions:**
14. Implied volatility dynamics and smile effects not incorporated in option pricing
15. Multiple settlement dates or partial hedging strategies not modeled
16. Cross-currency basis spreads ignored in money market calculations
17. Political or credit event risks excluded from scenario analysis

These assumptions establish a standardized analytical framework that another treasury analyst could replicate exactly using the same input data and methodology.

---

## 4. Calculation Flow

The model follows a structured sequence to compute USD proceeds under each hedging alternative. This section describes the logical order of operations without presenting specific formulas, enabling implementation in Excel or programmatic environments.

**Step 1: Forward Contract Hedge**
- Multiply the EUR receivable amount by the forward rate to calculate locked-in USD proceeds
- This represents the baseline "certainty" outcome with zero upfront cost
- Document the effective exchange rate and compare to current spot rate

**Step 2: Money Market Hedge (Synthetic Forward)**
- Calculate the present value of the EUR receivable by discounting at the EUR interest rate over the time period
- Determine the EUR amount to borrow today such that principal plus interest equals the receivable amount at maturity
- Convert the borrowed EUR amount to USD at the current spot rate
- Invest the USD proceeds at the USD interest rate for the time period
- Calculate the future value of the USD investment at maturity
- Verify that the receivable exactly repays the EUR loan (principal + interest)
- Compare the synthetic forward rate to the market forward rate as a validation check

**Step 3: EUR Put Option Hedge (Downside Protection)**
- Calculate the total premium cost by multiplying the per-unit premium by the receivable amount
- For each potential spot rate at maturity (S_T):
  - If S_T < K_put: Exercise the put option, receive USD proceeds at strike price
  - If S_T ≥ K_put: Let option expire, convert EUR at spot rate S_T
  - Subtract the upfront premium cost from gross proceeds to obtain net USD proceeds
- Document the break-even spot rate where net proceeds equal unhedged outcome

**Step 4: EUR Call Option Hedge (Alternative Structure)**
- Calculate the total premium cost by multiplying the per-unit premium by the receivable amount
- For each potential spot rate at maturity (S_T):
  - If S_T > K_call: Exercise the call option, receive USD proceeds at strike price
  - If S_T ≤ K_call: Let option expire, convert EUR at spot rate S_T
  - Subtract the upfront premium cost from gross proceeds to obtain net USD proceeds
- Note: This structure is typically used by importers (payables) but included for completeness

**Step 5: Unhedged Baseline**
- For each scenario spot rate at maturity, multiply EUR amount by S_T to calculate unhedged USD proceeds
- This serves as the benchmark for evaluating hedge effectiveness

**Step 6: Comparative Analysis**
- Create a comparison table showing USD proceeds under each strategy at the base-case forward rate
- Calculate the cost of hedging (opportunity cost vs. unhedged outcome at forward rate)
- Identify which strategy provides optimal risk-adjusted returns

**Step 7: Sensitivity Testing**
- Generate a matrix of outcomes across a range of terminal spot rates (S_T)
- For each rate scenario, compute USD proceeds under all hedge strategies
- Calculate the incremental value (gain/loss) of each hedge versus the unhedged position
- Visualize results to identify strategy dominance regions

**Sequencing Logic:**
The calculation flow begins with simple, deterministic strategies (forward and money market) before progressing to path-dependent outcomes (options). Each step builds on established variables from prior calculations, ensuring internal consistency. The sensitivity analysis integrates all prior calculations to provide comprehensive decision support.

---

## 5. Outputs

The model will generate the following outputs to support executive decision-making and provide comprehensive hedge evaluation:

| Output | Description | Format | Purpose |
|--------|-------------|--------|---------|
| **USD_forward** | USD proceeds from forward contract hedge | Single numeric value | Certainty benchmark showing locked-in outcome |
| **USD_mm** | USD proceeds from money market hedge | Single numeric value | Cross-check against forward for parity validation |
| **USD_put_table** | USD proceeds from EUR put option hedge across spot scenarios | Table (11 rows × 4 columns) | Sensitivity analysis showing downside protection |
| **USD_call_table** | USD proceeds from EUR call option hedge across spot scenarios | Table (11 rows × 4 columns) | Comparative analysis (primarily for educational purposes) |
| **Premium_costs** | Total upfront premium costs for each option strategy | Summary table | Cost comparison for option strategies |
| **Breakeven_rates** | Spot rates where option strategies break even vs. unhedged | Calculated values | Decision threshold identification |
| **Comparison_matrix** | All hedge outcomes at base-case forward rate | Summary table | Executive dashboard for strategy selection |
| **Sensitivity_table** | Complete sensitivity analysis: spot rates 1.00 to 1.20 | Table (11 rows × 7 columns) | Comprehensive outcome comparison across scenarios |
| **Chart_1** | USD proceeds vs. terminal spot rate (S_T) for all strategies | Multi-line chart | Visual comparison of hedge profiles |
| **Chart_2** | Incremental value of hedges vs. unhedged position | Multi-line chart | Cost-benefit visualization |
| **Summary_metrics** | Key statistics: min/max proceeds, downside protection, costs | Dashboard panel | Quick reference for decision-makers |
| **Recommendation** | Written analysis and hedge selection rationale | 2-3 paragraphs | Executive-ready takeaway and action plan |

**Output Specifications:**
- All monetary values rounded to nearest dollar for clarity
- Exchange rates displayed to four decimal places (e.g., 1.1607)
- Percentage values shown to two decimal places (e.g., 3.55%)
- Charts use professional color schemes with clear legends and axis labels
- Summary recommendation includes explicit hedge choice with supporting rationale

These outputs collectively provide a complete analytical package suitable for CFO presentation, board reporting, and treasury documentation requirements.

---

## 6. Sensitivity Plan

To evaluate the robustness of hedge recommendations across different exchange rate scenarios, the model will incorporate comprehensive sensitivity analysis:

**Scenario Range Definition:**
- Generate terminal spot rate scenarios (S_T) ranging from 1.0000 to 1.2000 USD/EUR
- Use increments of 0.02 (2 cents) to create 11 distinct scenarios
- This range represents approximately ±15% movement from the current forward rate
- Scenarios span from significant EUR weakness (1.00) to modest EUR strength (1.20)

**Calculation Methodology:**
For each spot rate scenario (S_T):
1. Calculate unhedged USD proceeds: FC_AMT × S_T
2. Calculate forward hedge proceeds: FC_AMT × F₀ (constant across all scenarios)
3. Calculate money market hedge proceeds: constant across all scenarios
4. Calculate put option net proceeds:
   - If S_T < K_put: FC_AMT × K_put - Premium_cost
   - If S_T ≥ K_put: FC_AMT × S_T - Premium_cost
5. Calculate call option net proceeds (for comparison):
   - If S_T > K_call: FC_AMT × K_call - Premium_cost
   - If S_T ≤ K_call: FC_AMT × S_T - Premium_cost

**Presentation Format:**
- **Sensitivity Table:** Rows represent spot rate scenarios, columns show proceeds under each hedge strategy plus delta comparisons
- **Line Chart:** X-axis shows terminal spot rates, Y-axis shows USD proceeds, separate lines for each hedge strategy
- **Highlight Analysis:** Identify crossover points where strategy preference changes

**Key Metrics to Extract:**
- Range of outcomes (min/max) under each strategy
- Downside protection level (minimum guaranteed proceeds)
- Upside participation (maximum potential proceeds)
- Break-even spot rates where hedge costs are justified
- Optimal strategy selection zones (rate ranges favoring each hedge)

**Visual Design Standards:**
- Use distinct colors for each strategy (Forward: Blue, Put Option: Purple, Money Market: Green, Unhedged: Gray)
- Mark the current forward rate (1.0875) with a vertical reference line
- Annotate break-even points and strategy dominance regions
- Include data labels for key intersections and extrema

This sensitivity framework enables stakeholders to understand not only the base-case recommendation but also how robust that recommendation is to different market outcomes, facilitating informed risk management decisions.

---

## 7. Limitations & Next Steps

**Analytical Limitations:**

This specification and resulting model incorporate several simplifications that should be acknowledged:

1. **Volatility Assumptions:** The model does not incorporate implied volatility surfaces or volatility smile effects in option pricing. Option premiums are taken as given market quotes rather than derived from stochastic models.

2. **Transaction Costs:** Bid-ask spreads, banking fees, and execution costs are excluded from the analysis. Real-world implementation would reduce net proceeds by 5-15 basis points depending on transaction size and counterparty relationships.

3. **Credit Risk:** Counterparty credit risk, collateral requirements, and credit valuation adjustments (CVA) are not modeled. Forward contracts may require ISDA agreements and margin posting.

4. **Liquidity Considerations:** The model assumes perfect market liquidity and ability to execute all strategies at quoted rates. Market depth and timing constraints are not evaluated.

5. **Tax and Accounting:** Tax implications of hedge gains/losses and hedge accounting treatment under ASC 815/IFRS 9 are excluded from the quantitative analysis.

6. **Partial Hedging:** The model evaluates 100% hedge ratios only. Partial hedging strategies (e.g., 50% forward + 50% option) are not explored.

7. **Dynamic Hedging:** The framework is static (single hedge decision at inception). Dynamic rebalancing or delta hedging strategies are not considered.

8. **Correlation Effects:** For multi-currency exposures, correlation benefits from portfolio diversification are not captured in this single-exposure analysis.

**Validation Requirements:**

Before model deployment, the following validation steps are necessary:
- Cross-check money market hedge calculation against forward rate to verify covered interest parity
- Confirm option break-even calculations algebraically
- Validate sensitivity table arithmetic across all scenarios
- Compare model outputs to Bloomberg FXHD function or similar professional tools

**Next Steps:**

**Stage 3 – Excel Model Build:** Implement this technical specification in a structured Excel workbook with three worksheets:
1. **Inputs:** Clean parameter entry with data validation and source documentation
2. **Calculations:** Formula logic for all three hedge strategies with intermediate steps visible
3. **Outputs:** Formatted results dashboard with comparison tables, sensitivity analysis, and professional charts

**Stage 4 – AI Prompt Engineering:** Develop a structured prompt that can generate this Excel model programmatically, testing reproducibility and automation potential for future hedge analyses.

**Stage 5 – Analysis & Recommendation:** Use the completed model to generate a final executive recommendation memo synthesizing quantitative results with qualitative strategic considerations.

**Timeline:** Complete Excel model build by October 30, 2025, to align with hedge execution deadline and CFO approval process.

---

## Appendix: Variable Reference Guide

**Quick Reference for Model Implementation:**

```
# Primary Inputs
FC_AMT = 4,500,000 EUR
S₀ = 1.1607 USD/EUR
F₀ = 1.0875 USD/EUR
r_USD = 3.55% per annum
r_EUR = 2.15% per annum
t = 1.0 years

# Option Parameters
K_put = 1.1607 USD/EUR (ATM)
K_call = 1.1607 USD/EUR (ATM)
Premium_put = 0.015 USD per EUR
Premium_call = 0.018 USD per EUR

# Sensitivity Range
S_T_min = 1.0000 USD/EUR
S_T_max = 1.2000 USD/EUR
S_T_increment = 0.0200 USD/EUR
```

This specification provides the complete analytical framework for building a professional-grade FX hedging decision model suitable for corporate treasury use.
