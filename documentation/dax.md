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
### Variance % Arrow Measures

```
Expense Variance % Arrow =
    VAR _upArrow = UNICHAR(129129)
    VAR _downArrow = UNICHAR(129131)
    RETURN
        IF(
            ISBLANK([Expense]),
            BLANK(),
            IF(
                [Expense Variance] > 0,
                "+" & ROUND([Expense Growth] * 100, 1) & "%" & _upArrow,
                ROUND([Expense Growth] * 100, 1) & "%" & _downArrow
            )
        )
Incoeme Variance % Arrow =
    VAR _upArrow = UNICHAR(129129)
    VAR _downArrow = UNICHAR(129131)
    RETURN
        IF(
            ISBLANK([Income]),
            BLANK(),
            IF(
                [Income Variance] > 0,
                "+" & ROUND([Income Growth] * 100, 1) & "%" & _upArrow,
                ROUND([Income Growth] * 100, 1) & "%" & _downArrow
            )
        )
```
The measures formats the month-over-month growth as a percentage with an upward or downward arrow for use in dashboard cards and indicators.

## Contribution Analysis

### Contribution

```
Contribution =
    VAR _allvalue =
        CALCULATE(
            [Total Amount],
            ALL(dim_Category[Type]),
            ALL(dim_Category[Category])
        )
    RETURN
        DIVIDE([Total Amount], _allvalue)
```
Calculates the contribution of the current category to the overall transaction value. The denominator removes the current Type and Category filters so the measure can compare the selected category against the overall total.

### Family & Friends Contribution

```
Family n Friends Contribution =
    VAR _allvalue =
        CALCULATE(
            [Total Amount],
            FILTER(
                ALL(dim_Recipient),
                dim_Recipient[RecipientName] <> BLANK()
            )
        )
    RETURN
        DIVIDE([Total Amount], _allvalue)
```
Calculates each recipient's contribution to the total transaction value across non-blank recipients, removing the existing recipient filter context before calculating the overall recipient total.

## Custom SVG Visualizations

### Contribution SVG

```
Contribution SVG =
    VAR _Percentage = [Contribution] * 100
    VAR _PercentageFormate = FORMAT(_Percentage, "#0")
    VAR _ProgressBar = _Percentage * 0.75

    RETURN
        "data:image/svg+xml;utf8," &
        "<svg width='120' height='30' xmlns='http://www.w3.org/2000/svg'
        xmlns:xlink='https://lnkd.in/dZ5ikEfb'
        display='block' overflow='visible'>

            <defs>
                <linearGradient id='gradient' x1='0%' y1='0%' x2='80%' y2='0%'
                gradientUnits='userSpaceOnUse'>
                    <stop offset='45%' style='stop-color: #24D1DB' />
                    <stop offset='130%' style='stop-color: #0FF14E'/>
                </linearGradient>
            </defs>

            <rect x='0' y='0' width='120' height='20'
            rx='10' ry='10' style='fill: #a0a7d8'/>

            <rect x='2.5' y='2.4' width='37' height='15'
            rx='8' ry='8' style='fill: #000000' />

            <text x='22' y='11.2' fill='white'
            text-anchor='middle' dominant-baseline='middle'
            font-family='Arial' font-weight='bold' font-size='12'>
                " & _PercentageFormate & "
                <tspan font-size='8' fill='white'> % </tspan>
            </text>

            <rect x='42.5' y='2.4' width='" & _ProgressBar & "'
            height='15' rx='8' ry='8' style='fill: url(#gradient)'>

                <animate attributeName='width'
                from='0' to='" & _ProgressBar & "'
                dur='2s' fill='freeze'/>

            </rect>
        </svg>"

```
This generates a dynamic SVG progress bar that displays each category's contribution percentage directly within the Power BI visual.
The SVG dimensions and progress-bar width are dynamically calculated from the [Contribution] measure, allowing the visual to update with the current filter context.

### Total Amount Area Sparkline

```
Total Amount Area Sparkline =
    VAR Defs =
        "<defs>
            <linearGradient id='grad'
                x1='0' y1='25'
                x2='0' y2='50'
                gradientUnits='userSpaceOnUse'>
                <stop stop-color='navy' offset='0' />
                <stop stop-color='transparent' offset='1' />
            </linearGradient>
        </defs>"

    VAR XMinDate =
        MIN('Calendar'[Monthnum])

    VAR XMaxDate =
        MAX('Calendar'[Monthnum])

    VAR YMinValue =
        MINX(
            VALUES('Calendar'[Monthnum]),
            CALCULATE([Total Amount])
        )

    VAR YMaxValue =
        MAXX(
            VALUES('Calendar'[Monthnum]),
            CALCULATE([Total Amount])
        )

    VAR SparklineTable =
        ADDCOLUMNS(
            SUMMARIZE(
                'Calendar',
                'Calendar'[Monthnum]
            ),
            "X",
                INT(
                    150 *
                    DIVIDE(
                        'Calendar'[Monthnum] - XMinDate,
                        XMaxDate - XMinDate
                    )
                ),
            "Y",
                INT(
                    50 *
                    DIVIDE(
                        [Total Amount] - YMinValue,
                        YMaxValue - YMinValue
                    )
                )
        )

    VAR Lines =
        CONCATENATEX(
            SparklineTable,
            [X] & "," & 50 - [Y],
            " ",
            'Calendar'[Monthnum]
        )

    VAR SVGImageURL =
        "data:image/svg+xml;utf8;" &
        "<svg xmlns='http://www.w3.org/2000/svg'
            x='0px' y='0px'
            viewBox='0 0 150 50'>" &
            Defs &
            "<polyline
                fill='url(%23grad)'
                fill-opacity='0.3'
                stroke='navy'
                stroke-width='3'
                points='0 50 " &
                Lines &
                " 150 50' />
            </svg>"

    RETURN
        SVGImageURL
```
This creates a dynamic area sparkline showing the monthly movement of total transaction value. The measure builds a temporary table of monthly values, scales the X and Y coordinates to the SVG dimensions, and generates the final SVG dynamically based on the current filter context.

