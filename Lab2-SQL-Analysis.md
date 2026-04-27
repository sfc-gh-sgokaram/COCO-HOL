# Lab 2: AI-Assisted SQL for Portfolio Analysis

**Time:** ~10 minutes  
**CoCo Feature:** AI Code Generation, SQL Iteration & Code Explanation  
**Learning Objective:** Use CoCo CLI to generate, iterate on, and explain SQL queries for portfolio analytics — and understand a complex pre-built query using AI.

---

## Background

Portfolio managers need rapid visibility into holdings composition, sector exposure, and performance vs benchmarks. CoCo acts as an on-demand SQL expert — generating, refining, and explaining complex financial queries from plain English directly in the terminal.

---

## Prerequisites

Complete **Lab 1** — your personal schema must contain all 4 tables with data.

If your CoCo session was closed, restart and re-set context:

```bash
cortex --connection <connection_name>
```

Then at the prompt:

```
/sql SET hol_database = '<YOUR DATABASE FOR HOL>'
/sql SET hol_warehouse = '<YOUR WAREHOUSE FOR HOL>'
/sql SET my_schema = '<YOUR SCHEMA FOR THE HOL>'
/sql USE DATABASE IDENTIFIER($hol_database)
/sql USE SCHEMA IDENTIFIER($my_schema)
/sql USE WAREHOUSE IDENTIFIER($hol_warehouse)
```

---

## Step 1 — Prompt 1: Sector Exposure Analysis

At the CoCo prompt, type:

### CoCo Prompt — Sector Exposure

```
Write and run a SQL query that shows sector exposure as a percentage of total portfolio
market value for portfolio_id = 'BLK_CORE_EQ'.

Use the PORTFOLIO_HOLDINGS table (already in my current schema context).

Show: sector, total_market_value, sector_weight_pct (rounded to 2dp), ticker_count.
Sort by sector_weight_pct descending.
```

CoCo will generate the SQL, execute it, and display results in the terminal.

---

## Step 2 — Iterate with a Follow-Up

Without any preamble, type directly at the CoCo prompt:

```
Modify the query to also compare against the S&P 500 benchmark.
Join to BENCHMARK_WEIGHTS (benchmark_name = 'S&P 500') and add:
- benchmark_weight_pct (sum of benchmark weights for that sector)
- active_weight_pct (portfolio weight minus benchmark weight, rounded to 2dp)
- Label each sector as OVERWEIGHT, UNDERWEIGHT, or NEUTRAL
  (overweight if active_weight > 2, underweight if < -2)

Save to lab2_analysis.sql and run it.
```

CoCo will modify the previously generated SQL and re-execute.

---

## Step 3 — Review Results

Review the output in the terminal. Discuss:
- Which sectors is BLK_CORE_EQ overweight vs S&P 500?
- Does the active weight calculation look correct?

---

## Step 4 — Explain the Complex Pre-Built Query

A pre-built complex query has been provided in `complex_query.sql` (in the same HOL directory as this lab).

> **Before starting this step:** Make sure `complex_query.sql` is in your current working directory. The facilitator will confirm this at the start of the session.

At the CoCo prompt, ask CoCo to explain it using the `@` file reference:

### CoCo Prompt — Explain the Query

```
Explain @complex_query.sql step by step. For each CTE, describe:
1. What it computes
2. Why it uses the window functions it does
3. How it feeds into the final SELECT

Also explain why NULLIF is used in the return calculation and what would happen without it.
```

CoCo reads `complex_query.sql` directly using the `@` file reference and explains it inline in the terminal — no copy-pasting required.

---

## Step 5 — Run the Complex Query

```
Now run @complex_query.sql and show the results.
```

You should see each holding with portfolio weight, benchmark weight, active weight classification, and 7-day return.

> **If `rolling_7d_return_pct` shows NULL for some rows**, ask:  
> `"Why might rolling_7d_return_pct be NULL for some rows in this result?"`

---

## Step 6 — (Optional / Stretch Goal)

```
Write and run a query that calculates monthly portfolio turnover for each portfolio_id
using the TRADE_HISTORY table.
Turnover = total BUY value in the month / average portfolio market value, as a percentage.
Show results for the last 3 months sorted by portfolio_id and month.
```

---

## Success Criteria

- [ ] Sector exposure query runs and returns results in the terminal
- [ ] Benchmark comparison iteration works correctly
- [ ] `complex_query.sql` created and CoCo explanation covers all 3 CTEs
- [ ] Complex query executes and returns results

---

## Key Takeaway

CoCo CLI acts as an always-available senior SQL engineer in your terminal. Whether generating new queries or demystifying inherited code with complex window functions and CTEs — all through natural language, without leaving the command line.

---

*Proceed to → [Lab 3: Portfolio Dashboard with Streamlit](./Lab3-Streamlit-Dashboard.md)*
