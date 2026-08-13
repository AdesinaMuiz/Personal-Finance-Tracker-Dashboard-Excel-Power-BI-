# Selected DAX Measures
Selected DAX calculations used in the Personal Finance Tracker Dashboard will be documented here, including their purpose, logic, and how they contribute to the analysis.

## 1. Calendar Table

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
