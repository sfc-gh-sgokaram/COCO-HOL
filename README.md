# Cortex Code (CoCo) Hands-On Lab

A self-contained Hands-On Lab (HOL) for introducing **Snowflake Cortex Code (CoCo)** to development teams. Participants use CoCo CLI to experience AI-assisted data engineering and analytics workflows through a Finance & Asset Management use case.

---

## What is This HOL?

This HOL walks participants through three progressive lab exercises using CoCo CLI — Snowflake's AI-powered coding assistant — entirely from the terminal. No prior SQL or Python expertise is required to complete the labs.

| Lab | Title | What You'll Do |
|---|---|---|
| Lab 1 | AI-Assisted Data Generation | Use CoCo to generate a 4-table Finance schema and seed it with realistic data |
| Lab 2 | AI-Assisted SQL Analysis | Use CoCo to write, iterate on, and explain complex portfolio analytics queries |
| Lab 3 | Portfolio Dashboard (Streamlit) | Use CoCo to generate and deploy a Streamlit in Snowflake dashboard |

**Total duration:** ~35 minutes  
**Domain:** Finance & Asset Management (portfolio holdings, prices, benchmarks, trades)

---

## Repository Structure

```
COCO-HOL/
├── .gitignore
├── README.md                       ← You are here
├── Lab1-Data-Generation.md         ← Lab 1 instructions + CoCo prompts
├── Lab2-SQL-Analysis.md            ← Lab 2 instructions + CoCo prompts
├── Lab3-Streamlit-Dashboard.md     ← Lab 3 instructions + CoCo prompts
└── complex_query.sql               ← Pre-built query used in Lab 2 Step 4
```

---

## Prerequisites

Before starting, ensure you have:

- A Snowflake account provisioned by the facilitator
- CoCo CLI installed (`cortex --version` should return a version number)
- A configured Snowflake connection (provided by the facilitator)

### Verify CoCo CLI is installed

```bash
cortex --version
```

If not installed, follow the [Cortex Code CLI setup guide](https://docs.snowflake.com/en/user-guide/cortex-code/cortex-code-cli).

---

## How to Run the HOL

### Step 1 — Get the values from your facilitator

Before starting, note down:
- **HOL Database name** (e.g. `BLACKROCK_HOL_DB`)
- **HOL Warehouse name** (e.g. `HOL_WH`)
- **Snowflake connection name** (e.g. `hol_connection`)

### Step 2 — Clone or download this repository

```bash
git clone <repo_url>
cd COCO-HOL
```

### Step 3 — Launch CoCo CLI

```bash
cortex --connection <connection_name>
```

### Step 4 — Set your session context

At the CoCo prompt, run (replace values with what your facilitator announced):

```
/sql SET hol_database  = 'BLACKROCK_HOL_DB'
/sql SET hol_warehouse = 'HOL_WH'
/sql SET my_schema     = LOWER(CURRENT_USER()) || '_hol'
/sql USE DATABASE  IDENTIFIER($hol_database)
/sql USE SCHEMA    IDENTIFIER($my_schema)
/sql USE WAREHOUSE IDENTIFIER($hol_warehouse)
```

### Step 5 — Work through the labs in order

1. [Lab 1 — Data Generation](./Lab1-Data-Generation.md)
2. [Lab 2 — SQL Analysis](./Lab2-SQL-Analysis.md)
3. [Lab 3 — Streamlit Dashboard](./Lab3-Streamlit-Dashboard.md)

Each lab is self-contained with CoCo prompts and step-by-step instructions.

---

## Multi-User Design

Each participant works in an **isolated personal schema** named `<your_snowflake_username>_hol`. All tables, queries, and apps you create live in your schema — no participant can affect another's work.

```
HOL_DATABASE/
├── jsmith_hol/     ← Participant: jsmith (your tables + Streamlit app)
├── alee_hol/       ← Participant: alee
└── mwilson_hol/    ← Participant: mwilson
```

---

## For Facilitators

See the **Facilitator Guide** section in `HOL-Specifications.md` (not tracked in git) for:
- Pre-lab environment setup and grants
- Timing guide
- Troubleshooting tips
- Post-session cleanup script

---

## Key CoCo CLI Tips

| What you want | How to do it |
|---|---|
| Run a quick SQL statement | `/sql <your query>` |
| Reference a local file | `@filename.sql` in your prompt |
| Run a bash command | `!<command>` |
| Undo the last action | `/rewind` |
| Compact long sessions | `/compact` |
