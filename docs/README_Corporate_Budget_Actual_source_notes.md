# Corporate Budget vs. Actual Analysis Dashboard

## Project Overview

This project analyzes three years of corporate expense data in Tableau to evaluate spending patterns, budget performance, and the operational drivers behind budget overruns.

The analysis is structured around two questions:

1. **Where is the company spending money?**
2. **How does actual spending compare with the approved budget?**

The Tableau workbook combines high-level budget-versus-actual reporting with department, category, region, transaction, and variance analysis. The project is designed to reflect the type of recurring expense reporting and variance investigation performed in FP&A, corporate finance, and business operations roles.

## Tools Used

- **Microsoft Excel** — source-data review and cleaning
- **Tableau Public / Tableau Desktop Public Edition** — data visualization, calculated fields, filters, and dashboard development

## Dataset

The dataset contains simulated corporate transactions from **January 1, 2021 through December 31, 2023**.

### Final Dataset Structure

| Field | Description |
|---|---|
| Date | Transaction date |
| Department | Finance, HR, IT, Marketing, Operations, or Sales |
| Category | Infrastructure, Marketing, Salaries, Training, Travel, or Utilities |
| Region | Central, East, North, South, or West |
| Budget Amount | Planned transaction amount |
| Actual Amount | Recorded transaction amount |
| Payment Method | Bank Transfer, Card, Cash, or UPI |
| Transaction ID | Unique transaction identifier |

After cleaning, the analytical dataset contains:

- **9,995 transactions**
- **9,995 unique transaction IDs**
- **6 departments**
- **6 expense categories**
- **5 regions**
- **4 payment methods**
- **3 full calendar years**

## Data Cleaning Process

The source file was reviewed and cleaned in Excel before being connected to Tableau.

### 1. Missing-Value Review

Rows containing blanks in required analytical fields were removed because they could not support reliable department, category, region, date, budget, or actual-spend analysis.

Three otherwise complete records were missing only their transaction identifiers. Rather than deleting valid financial records, unique surrogate IDs were assigned:

- `TXN110000`
- `TXN110001`
- `TXN110002`

These identifiers were created only to preserve uniqueness and record traceability; they were not present in the original source.

### 2. Duplicate Removal

The source contained **10 exact duplicate records**. Each duplicate matched its paired record across all eight fields:

- Date
- Department
- Category
- Region
- Budget Amount
- Actual Amount
- Payment Method
- Transaction ID

One copy of each duplicated record was removed using Excel's **Remove Duplicates** function with all columns selected.

The affected transaction IDs were:

- `TXN101933`
- `TXN101989`
- `TXN102041`
- `TXN103850`
- `TXN103886`
- `TXN104962`
- `TXN105437`
- `TXN108517`
- `TXN109953`
- `TXN109984`

This reduced the dataset from **10,005 rows to 9,995 unique transaction records**.

### 3. Data-Type Validation

The following field types were validated before analysis:

- `Date` — date
- `Budget Amount` — numeric
- `Actual Amount` — numeric
- `Department`, `Category`, `Region`, and `Payment Method` — categorical text
- `Transaction ID` — text identifier

The date range was validated as **2021-01-01 through 2023-12-31**.

### 4. Uniqueness and Completeness Checks

The cleaned dataset was checked to confirm:

- Every remaining transaction ID is unique
- No duplicate full rows remain
- Budget and actual amounts are populated and numeric
- Department, category, and region values use consistent labels
- The final row count matches the unique transaction count

## Tableau Calculated Fields

### Variance $

For an expense dataset, positive variance indicates actual spending exceeded budget.

```tableau
[Actual Amount] - [Budget Amount]
```

Interpretation:

- Positive = over budget / unfavorable
- Negative = under budget / favorable
- Zero = exactly on budget

### Variance %

```tableau
([Actual Amount] - [Budget Amount]) / [Budget Amount]
```

This standardizes the size of a miss relative to the original budget.

### Over Budget Flag

```tableau
IF [Actual Amount] > [Budget Amount] THEN 1
ELSE 0
END
```

### Over Budget Rate

Because the cleaned dataset contains one row per transaction, the average of the binary flag equals the percentage of transactions that exceeded budget.

```tableau
AVG([Over Budget Flag])
```

An equivalent aggregate calculation is:

```tableau
SUM([Over Budget Flag]) / COUNTD([Transaction ID])
```

## Analysis and Visuals

### Corporate Spending Overview

#### Monthly Budget vs. Actual Spend

A monthly line chart compares total budget and actual spending over the full 36-month period.

**Purpose:** Identify spending trends, recurring peaks, and periods where actual spending diverged from plan.

#### Budget vs. Actual by Department

Side-by-side bars compare planned and actual expense totals across departments.

**Purpose:** Show which organizational functions account for the most spending and where actual results exceed plan.

#### Budget vs. Actual by Expense Category

Side-by-side horizontal bars compare budget and actual amounts for each expense category.

**Purpose:** Identify the cost types driving overall expenditure and budget pressure.

#### Budget vs. Actual by Region

Side-by-side bars compare spending across Central, East, North, South, and West.

**Purpose:** Evaluate geographic differences without using a misleading map for non-specific directional regions.

#### Department Spend Drivers: Volume vs. Average Transaction Value

Each point represents one department.

- X-axis: distinct transaction count
- Y-axis: average actual transaction amount
- Bubble size: total actual spending
- Label: department

**Purpose:** Separate the two mechanical drivers of total spending:

> Total Spend ≈ Transaction Volume × Average Transaction Value

This helps determine whether a department spends more because it processes more transactions, has larger transactions, or both.

#### Department Expense Mix

A department-by-category highlight table displays actual spending at the intersection of each department and expense category.

**Purpose:** Reveal the expense categories driving each department's spending profile.

#### Largest Individual Transactions

A detail table displays the ten transactions with the highest actual amounts, including transaction ID, date, department, category, region, and actual amount.

**Purpose:** Provide transaction-level drill-down and identify the records with the greatest individual financial impact.

### Budget Variance Analysis

#### Monthly Budget Variance

Monthly bars display:

```text
Actual Amount - Budget Amount
```

**Purpose:** Show when the largest aggregate budget misses occurred.

#### Budget Variance by Department

Departments are ranked by total dollar variance.

**Purpose:** Identify the departments contributing the most to the company's overall budget overrun.

#### Over-Budget Rate by Department

Departments are ranked by the percentage of transactions where actual spending exceeded budget.

**Purpose:** Distinguish departments with frequent budget misses from departments whose total variance may be driven by fewer, larger transactions.

## Key Findings

Across the cleaned dataset:

- **Total Budget:** $795.1 million
- **Total Actual Spend:** $889.8 million
- **Unfavorable Variance:** $94.6 million
- **Aggregate Variance:** 11.9% above budget
- **Transactions Over Budget:** 5,587
- **Transactions Under Budget:** 4,408
- **Overall Over-Budget Rate:** 55.9%
- **Average Actual Transaction:** approximately $89,020
- **Largest Actual Transaction:** $169,973

### Annual Performance

| Year | Budget | Actual | Variance | Variance % |
|---|---:|---:|---:|---:|
| 2021 | $256.6M | $287.4M | $30.8M | 12.0% |
| 2022 | $266.3M | $302.6M | $36.3M | 13.6% |
| 2023 | $272.2M | $299.8M | $27.6M | 10.1% |

**2022 produced the largest total budget overrun and the highest variance percentage.**

### Department Findings

- **HR recorded the highest actual spending:** $155.4M
- **Marketing generated the largest unfavorable department variance:** $19.9M
- **Marketing also had the highest over-budget transaction rate:** approximately 57.5%
- **Finance produced the smallest department variance:** $11.1M

This demonstrates why actual spending and variance should be analyzed separately. HR spent the most overall, but Marketing created the largest gap relative to plan.

### Category Findings

- **Utilities recorded the highest actual spending:** $155.3M
- **Salaries generated the largest category variance:** $20.6M
- **Salaries finished approximately 16.2% above budget**

### Regional Findings

- **Central recorded the highest actual spending:** $182.0M
- **East generated the largest regional variance:** $21.4M
- **East finished approximately 13.4% above budget**

## Dashboard Interactivity

The workbook supports filtering by:

- Year
- Department
- Category
- Region

Department, category, and region visuals can also be configured as dashboard filter actions, allowing users to move from a company-level view into a more focused analysis.

Tooltips provide supporting measures such as:

- Budget amount
- Actual amount
- Variance $
- Variance %
- Transaction count
- Average transaction value

## Design Decisions

- Actual and budget are shown together where direct comparison improves the analysis.
- Variance is displayed separately when the difference itself is the primary question.
- Payment method was excluded from the main dashboard because it did not produce a meaningful FP&A insight.
- A region bar chart was used instead of a map because the dataset contains directional regions rather than specific geographic locations.
- A detail table was used for the largest transactions instead of another bar chart to support record-level review.
- Department labels replace an unnecessary color legend in the transaction-driver scatterplot.
- The analysis avoids overstating small differences in a synthetically balanced dataset.

## Limitations

- The dataset is simulated and does not represent the financial records of a real company.
- Budget values exist at the transaction level, whereas real FP&A budgets are often maintained by cost center, account, department, and reporting period.
- Every month in the cleaned dataset aggregates above budget, suggesting the source data is structurally biased toward overspending.
- The dataset does not include vendor, account, cost-center owner, revenue, headcount, or forecast data.
- Regional labels are directional and cannot support precise geographic analysis.
- Payment method is available but has limited relevance to budget-performance decision-making.
- Findings should be interpreted as a demonstration of financial-analysis and Tableau skills rather than as recommendations for an actual organization.

## Repository Structure

```text
corporate-budget-actual-tableau/
├── data/
│   └── Budget_vs_Actual_Data_clean.xlsx
├── tableau/
│   └── Corporate_Budget_Actual_Analysis.twbx
├── images/
│   └── dashboard-preview.png
└── README.md
```

## How to Use

1. Download the cleaned Excel dataset.
2. Open the packaged Tableau workbook.
3. Use the year, department, category, and region filters to explore the data.
4. Select marks in the department, category, or region charts to drill into related views.
5. Hover over marks to review budget, actual, variance, and transaction-level details.

## Project Purpose

This project demonstrates:

- Financial data cleaning and validation
- Budget-versus-actual analysis
- Expense and variance reporting
- KPI and calculated-field development
- Tableau dashboard design
- Interactive filtering and drill-down
- Translation of transaction-level data into management-ready insights
