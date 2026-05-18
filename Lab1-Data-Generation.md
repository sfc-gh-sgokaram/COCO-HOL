# Lab 1: AI-Assisted Data Generation

**Time:** ~10 minutes  
**CoCo Feature:** AI Code Generation — DDL & Data Seeding  
**Learning Objective:** Use CoCo CLI to generate table schemas and realistic sample data for a Finance Asset Management domain — without writing a single line of SQL manually.

---

## Background

Before any analytics can happen, data engineers need to scaffold data models and seed them with representative test data. This is tedious to do by hand for multi-table relational schemas. CoCo can generate the entire setup — DDL + data — from a concise natural language prompt.

---

## Prerequisites

Complete Instructions in README before starting. You should have your Snowflake connection configured , Database and Schema. 

---

## Step 1 — Launch CoCo CLI

Open your terminal and start CoCo pointing to your HOL connection:

```bash
cortex --connection <connection_name>
```

> The facilitator will provide the connection name to use.

---

## Step 2 — Set Your Working Context

At the CoCo prompt, run the following SQL to set your session context. Replace the values with what the facilitator announced:

```
/sql SET hol_database = '<YOUR DATABASE FOR HOL>'
/sql SET hol_warehouse = '<YOUR WAREHOUSE FOR HOL>'
/sql SET my_schema = '<YOUR SCHEMA FOR THE HOL>'
/sql USE DATABASE IDENTIFIER($hol_database)
/sql USE SCHEMA IDENTIFIER($my_schema)
/sql USE WAREHOUSE IDENTIFIER($hol_warehouse)
```

Verify:

```
/sql SELECT 'Database: ' || $hol_database || ' | Schema: ' || $my_schema || ' | Warehouse: ' || $hol_warehouse AS status
```

---

## Step 3 — Send the Data Generation Prompt

At the CoCo prompt, type the following:

---

### CoCo Prompt

```
I'm building a Finance Asset Management demo in Snowflake.
My session is already pointing to the right database and schema — do not hardcode any names.

Create these 4 tables (DROP IF EXISTS so the script is re-runnable):
- PORTFOLIO_HOLDINGS: tracks portfolio positions (portfolio_id, asset_id, ticker, asset_class, sector, quantity, market_value_usd, as_of_date)
- ASSET_PRICES: daily price history (asset_id, ticker, price_date, close_price, volume)
- BENCHMARK_WEIGHTS: S&P 500 index weights (benchmark_id, benchmark_name, ticker, weight_pct, as_of_date)
- TRADE_HISTORY: buy/sell trades (trade_id, portfolio_id, ticker, trade_type, quantity, trade_price, trade_date)

Then insert ~100 rows of realistic data into each table:
- Use real tickers across these sectors — Technology: AAPL, MSFT, GOOGL, AMZN, NVDA, META, TSLA | Financials: JPM, BAC, GS, V, MA | Healthcare: JNJ, UNH, PFE | Energy: XOM, CVX | Consumer Staples: PG, KO, WMT
- 3 portfolio IDs: BLK_CORE_EQ, BLK_GROWTH, BLK_VALUE
- ASSET_PRICES: 30 days of price history ending today with realistic prices and ~2% daily variation
- BENCHMARK_WEIGHTS: weights must sum to exactly 100 per as_of_date
- TRADE_HISTORY: mix of BUY/SELL trades over the last 90 days

Save the SQL to a file called lab1_setup.sql and execute it.
Then verify with:
SELECT 'PORTFOLIO_HOLDINGS' AS tbl, COUNT(*) AS rows FROM PORTFOLIO_HOLDINGS UNION ALL
SELECT 'ASSET_PRICES',              COUNT(*)             FROM ASSET_PRICES      UNION ALL
SELECT 'BENCHMARK_WEIGHTS',         COUNT(*)             FROM BENCHMARK_WEIGHTS UNION ALL
SELECT 'TRADE_HISTORY',             COUNT(*)             FROM TRADE_HISTORY;
```

CoCo will generate `lab1_setup.sql`, Review the SQL and valdiate.  Once done then ask CoCo to execute the SQL and create the objects. 

---

## Step 4 — Check the Verification Output

You should see output like:

```
┌────────────────────┬──────┐
│ TBL                │ ROWS │
├────────────────────┼──────┤
│ PORTFOLIO_HOLDINGS │  100 │
│ ASSET_PRICES       │  100 │
│ BENCHMARK_WEIGHTS  │   20 │
│ TRADE_HISTORY      │  100 │
└────────────────────┴──────┘
```

> **If something looks off**, follow up directly in the CoCo prompt:  
> `"The benchmark weights don't sum to 100 per date — please correct the weight_pct values and re-run."`

CoCo will edit `lab1_setup.sql` and re-execute without you needing to touch the file.

---

## Step 5 — Iterate: Add a Data Quality Check

Type the following at the CoCo prompt:

```
Write and run a query that validates the data we just loaded:
1. Confirm BENCHMARK_WEIGHTS sums to 100 per as_of_date
2. Check for any tickers in PORTFOLIO_HOLDINGS that have no price data in ASSET_PRICES
3. Check for any TRADE_HISTORY records where trade_price is 0 or NULL
```

This shows how CoCo pivots immediately from generating data to auditing it — all within the same terminal session.

---

## Success Criteria

- [ ] All 4 tables created in your personal schema
- [ ] Each table contains ~100 rows of realistic data
- [ ] Verification output shows row counts > 0 for all tables
- [ ] Data quality check runs and returns results

---

## Key Takeaway

CoCo CLI eliminates hours of manual DDL and data scaffolding. A complete 4-table schema with realistic Finance domain data was generated, executed, and validated entirely through natural language — without leaving the terminal.

---

*Proceed to → [Lab 2: AI-Assisted SQL Analysis](./Lab2-SQL-Analysis.md)*
