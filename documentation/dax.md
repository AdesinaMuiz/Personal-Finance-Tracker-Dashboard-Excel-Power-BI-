# Selected DAX Measures
Selected DAX calculations used in the Personal Finance Tracker Dashboard will be documented here, including their purpose, logic, and how they contribute to the analysis.

## Calendar Table

```
Calendar =
    ADDCOLUMNS(
        CALENDAR(
            MIN(Fact_Finance[Date]),
            MAX(Fact_Finance[Date])
        ),
        "Year", YEAR([Date]),
        "Month", FORMAT([Date], "mmm"),
        "Monthnum", MONTH([Date]),
        "Weekday", FORMAT([Date], "ddd"),
        "Weeknum", WEEKDAY([Date]),
        "Qtr", "Q" & FORMAT([Date], "Q"),
        "WeekType",
            IF(
                WEEKDAY([Date]) = 1 || WEEKDAY([Date]) = 7,
                "Weekend",
                "Weekday"
            )
    )
  ```
This creates a dedicated calendar table covering the full transaction period, with fields for year, month, numeric month ordering, weekday, numeric day-of-week, quarter, and weekday/weekend classification. The table is related to Fact_Finance[Date] and supports consistent date filtering and time-based analysis across the dashboard.


### Total Amount

```
Total Amount = SUM(Fact_Finance[Amount])
```
Calculates the total transaction value within the current filter context.

### Total Balance

```
Total Balance = SUM(dim_Account[Balance])
```
Calculates the total balance across accounts in the current filter context.

### Income

```
Income =
    CALCULATE(
        [Total Amount],
        dim_Category[Type] = "Income"
    )
```
Calculates total transaction value classified as income within the current filter context.

### Expense

```
Expense =
    CALCULATE(
        [Total Amount],
        dim_Category[Type] = "Expense"
    )
```
Calculates total transaction value classified as expenses within the current filter context.

### Expense Neg

```
Expense Neg =
    CALCULATE(
        [Total Amount],
        dim_Category[Type] = "Expense"
    ) * -1
```
Returns expenses as negative values to support visuals and calculations where expenses need to be displayed as deductions from income.

### Expense %

```
Expense % = DIVIDE([Expense], [Total Amount], 0)
```
Calculates expenses as a proportion of total transaction value, returning 0 when the denominator is zero.

### Net Income

```
Net Income = [Income] - [Expense]
```
Calculates the amount remaining after expenses are deducted from total income.

### Previous Month Measures

```
Expense PM =
    CALCULATE(
        [Expense],
        DATEADD('Calendar'[Date], -1, MONTH)
    )

Income PM =
    CALCULATE(
        [Income],
        DATEADD('Calendar'[Date], -1, MONTH)
    )

Net Income PM =
    CALCULATE(
        [Net Income],
        DATEADD('Calendar'[Date], -1, MONTH)
    )
```
These measures return the corresponding financial metric for the previous month, using the Calendar table to shift the current date context back by one month.

### Variance & Growth Measures

```
Expense Variance = [Expense] - [Expense PM]

Income Variance = [Income] - [Income PM]

Net Income Variance = [Net Income] - [Net Income PM]

Expense Growth = DIVIDE([Expense Variance], [Expense])

Income Growth = DIVIDE([Income Variance], [Income])

Net Income Growth = DIVIDE([Net Income Variance], [Net Income])
```
