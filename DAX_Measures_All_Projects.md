# DAX Measures Reference — All 5 Power BI Projects
**Author:** Zulfiqar Ahmed Huq | WFM Analytics & Reporting

---

## PROJECT 1 — Agent Productivity & Utilization Dashboard

### Basic Aggregations
```dax
Total Handle Time =
SUM(AgentActivity[HandleTime_sec])

Total Idle Time =
SUM(AgentActivity[IdleTime_sec])

Total Available Time =
SUM(AgentActivity[AvailableTime_sec])

Total Calls Handled =
SUM(AgentActivity[CallsHandled])
```

### Core KPIs
```dax
Occupancy Rate =
DIVIDE([Total Handle Time], [Total Available Time], 0)

AHT (seconds) =
DIVIDE([Total Handle Time], [Total Calls Handled], 0)

AHT (minutes) =
DIVIDE([AHT (seconds)], 60, 0)

Active Time % =
DIVIDE([Total Handle Time], [Total Handle Time] + [Total Idle Time], 0)

Idle Time % =
1 - [Active Time %]
```

### Productivity Score (Composite)
```dax
Productivity Score =
VAR OccScore   = [Occupancy Rate] * 0.4
VAR AHTFactor  = DIVIDE(AVERAGE(AgentActivity[Target_Occupancy]), [Occupancy Rate], 0)
VAR ActiveScore = [Active Time %] * 0.3
RETURN
    ROUND((OccScore + AHTFactor * 0.3 + ActiveScore), 2)
```

### Trend (Week-over-Week)
```dax
Occupancy WoW Change =
[Occupancy Rate] -
CALCULATE(
    [Occupancy Rate],
    DATEADD(Dates[Date], -7, DAY)
)

Occupancy WoW Arrow =
IF([Occupancy WoW Change] > 0, "▲", IF([Occupancy WoW Change] < 0, "▼", "–"))
```

### vs Target
```dax
Occupancy vs Target =
[Occupancy Rate] - AVERAGE(AgentActivity[Target_Occupancy])

Occupancy Status =
IF([Occupancy vs Target] >= 0, "On Target ✓", "Below Target ⚠")
```

---

## PROJECT 2 — Service Level Goals Tracking Report

### Basic Aggregations
```dax
Total Offered Calls =
SUM(ServiceLevel[OfferedCalls])

Total Answered in Threshold =
SUM(ServiceLevel[AnsweredInThreshold])

Total Abandoned =
SUM(ServiceLevel[AbandonedCalls])
```

### Core KPIs
```dax
SVL % =
DIVIDE([Total Answered in Threshold], [Total Offered Calls], 0)

Abandon Rate =
DIVIDE([Total Abandoned], [Total Offered Calls], 0)

SVL Target =
AVERAGE(ServiceLevel[SVL_Target])

SVL Gap =
[SVL %] - [SVL Target]

SVL Status =
IF([SVL Gap] >= 0, "✓ Met", "⚠ Missed")
```

### Period Comparisons
```dax
SVL DoD Change =
[SVL %] -
CALCULATE([SVL %], DATEADD(Dates[Date], -1, DAY))

SVL WoW Change =
[SVL %] -
CALCULATE([SVL %], DATEADD(Dates[Date], -7, DAY))

SVL MoM Change =
[SVL %] -
CALCULATE([SVL %], DATEADD(Dates[Date], -1, MONTH))
```

### Trend Color Flag
```dax
SVL Color =
SWITCH(
    TRUE(),
    [SVL %] >= 0.90, "Green",
    [SVL %] >= 0.80, "Amber",
    "Red"
)
```

---

## PROJECT 3 — Staffing Compliance & Quality Checks

### Headcount Measures
```dax
Total Rostered HC =
SUM(Staffing[RosteredHC])

Total Actual HC =
SUM(Staffing[ActualHC])

HC Variance =
[Total Actual HC] - [Total Rostered HC]

HC Variance % =
DIVIDE([HC Variance], [Total Rostered HC], 0)
```

### Compliance Measures
```dax
Compliance Rate =
DIVIDE(
    COUNTROWS(FILTER(Staffing, Staffing[ActualHC] >= Staffing[RosteredHC])),
    COUNTROWS(Staffing),
    0
)

Avg Schedule Adherence =
AVERAGE(Staffing[ScheduleAdherence_pct])

Compliance Flag =
IF([HC Variance] >= 0, "✓ Compliant", "⚠ Understaffed")

Compliance Target Gap =
[Compliance Rate] - AVERAGE(Staffing[ComplianceTarget])
```

### Quality
```dax
Avg Quality Score =
AVERAGE(Staffing[QualityScore])

Quality Rating =
SWITCH(
    TRUE(),
    [Avg Quality Score] >= 4.5, "Excellent",
    [Avg Quality Score] >= 4.0, "Good",
    [Avg Quality Score] >= 3.5, "Average",
    "Needs Improvement"
)
```

### WoW Trend
```dax
Compliance WoW =
[Compliance Rate] -
CALCULATE(
    [Compliance Rate],
    DATEADD(Dates[Date], -7, DAY)
)
```

---

## PROJECT 4 — Mortgage Market & Portfolio Comparison

### Volume Measures
```dax
Total Market Volume =
SUM(Mortgage[MarketVolume_M])

Total Client Volume =
SUM(Mortgage[ClientVolume_M])

Market Share % =
DIVIDE([Total Client Volume], [Total Market Volume], 0)
```

### Rate Measures
```dax
Market Avg Rate =
AVERAGE(Mortgage[MarketAvgRate])

Client Avg Rate =
AVERAGE(Mortgage[ClientAvgRate])

Rate Differential =
[Client Avg Rate] - [Market Avg Rate]

Rate Differential Label =
IF(
    [Rate Differential] > 0,
    "Client Above Market ▲",
    "Client Below Market ▼"
)
```

### Risk Measures
```dax
Market Default Rate =
AVERAGE(Mortgage[MarketDefaults_pct])

Client Default Rate =
AVERAGE(Mortgage[ClientDefaults_pct])

Default Risk Gap =
[Client Default Rate] - [Market Default Rate]

Risk Flag =
IF([Default Risk Gap] > 0.01, "⚠ Elevated Risk", "✓ Within Range")
```

### YoY Growth
```dax
Client Volume YoY =
DIVIDE(
    [Total Client Volume] -
        CALCULATE([Total Client Volume], SAMEPERIODLASTYEAR(Dates[Date])),
    CALCULATE([Total Client Volume], SAMEPERIODLASTYEAR(Dates[Date])),
    0
)

Market Volume YoY =
DIVIDE(
    [Total Market Volume] -
        CALCULATE([Total Market Volume], SAMEPERIODLASTYEAR(Dates[Date])),
    CALCULATE([Total Market Volume], SAMEPERIODLASTYEAR(Dates[Date])),
    0
)
```

---

## PROJECT 5 — Retail Sales & Profit Analytics

### Basic Aggregations
```dax
Total Sales =
SUM(Sales[Sales])

Total Profit =
SUM(Sales[Profit])

Total Quantity =
SUM(Sales[Quantity])

Total Orders =
COUNTROWS(Sales)
```

### Profitability
```dax
Profit Margin % =
DIVIDE([Total Profit], [Total Sales], 0)

Avg Discount =
AVERAGE(Sales[Discount])

Avg Order Value =
DIVIDE([Total Sales], [Total Orders], 0)
```

### YoY Comparisons
```dax
Sales LY =
CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Dates[Date]))

Sales YoY % =
DIVIDE([Total Sales] - [Sales LY], [Sales LY], 0)

Profit LY =
CALCULATE([Total Profit], SAMEPERIODLASTYEAR(Dates[Date]))

Profit YoY % =
DIVIDE([Total Profit] - [Profit LY], [Profit LY], 0)

YoY Sales Arrow =
IF([Sales YoY %] > 0, "▲ " & FORMAT([Sales YoY %], "0.0%"),
   "▼ " & FORMAT([Sales YoY %], "0.0%"))
```

### Discount Impact (Regression-based)
```dax
-- Estimated profit impact of discount per order
Discount Impact on Profit =
SUMX(
    Sales,
    Sales[Sales] * Sales[Discount] * -1.5
)

Discount Sensitivity =
DIVIDE([Discount Impact on Profit], [Total Sales], 0)
```

### RLS Support
```dax
-- Region filter for RLS role
-- Applied in Manage Roles: [Region] = USERPRINCIPALNAME()
-- Verify with: USERPRINCIPALNAME()
```

### Rankings
```dax
Product Rank by Sales =
RANKX(
    ALL(Sales[SubCategory]),
    [Total Sales],
    ,
    DESC,
    DENSE
)

Top 5 Flag =
IF([Product Rank by Sales] <= 5, "Top 5", "Others")
```

---

## SHARED — Date Table (use in all projects)

```dax
Dates =
ADDCOLUMNS(
    CALENDAR(DATE(2022,1,1), DATE(2024,12,31)),
    "Year",       YEAR([Date]),
    "Month",      MONTH([Date]),
    "MonthName",  FORMAT([Date], "MMM"),
    "Quarter",    "Q" & QUARTER([Date]),
    "WeekNum",    WEEKNUM([Date]),
    "DayName",    FORMAT([Date], "DDD"),
    "YearMonth",  FORMAT([Date], "YYYY-MM"),
    "IsWeekend",  IF(WEEKDAY([Date],2) >= 6, TRUE, FALSE)
)
```
