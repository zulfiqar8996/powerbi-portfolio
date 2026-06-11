# Power BI Dashboard Layout Guide — All 5 Projects
**Author:** Zulfiqar Ahmed Huq

---

## HOW TO USE THIS GUIDE
1. Open Power BI Desktop
2. Import the matching CSV file from the `data/` folder
3. Create a Date table using the DAX in `DAX_Measures_All_Projects.md`
4. Build each page following the layout below
5. Save as `.pbit` (template) for GitHub upload

---

## PROJECT 1 — Agent Productivity & Utilization Dashboard
**CSV:** `01_agent_productivity.csv`

### Data Model
- Fact table: `AgentActivity`
- Dimension: `Dates` (mark as Date Table on `Date` column)
- Dimension: `LOB` (create from distinct values)

### Pages & Visuals

#### Page 1 — Executive Summary
| Position | Visual | Fields |
|----------|--------|--------|
| Top row (3 cards) | KPI Cards | Occupancy Rate · AHT (min) · Active Time % |
| Middle left | Line Chart | Occupancy Rate by Date |
| Middle right | Bar Chart | Calls Handled by LOB |
| Bottom | Table | Agent · Occupancy · AHT · Status |

#### Page 2 — Agent Drill-Down
| Position | Visual | Fields |
|----------|--------|--------|
| Top | Slicer | LOB · Date Range |
| Left | Bar Chart | Occupancy Rate by AgentID (Top 10) |
| Right | Scatter Plot | AHT vs Occupancy by Agent |
| Bottom | Matrix | AgentID × LOB × Occupancy Rate |

#### Page 3 — Trend Analysis
| Position | Visual | Fields |
|----------|--------|--------|
| Full width | Line Chart | Occupancy Rate + Target line by Week |
| Bottom left | Column Chart | Idle vs Active Time by LOB |
| Bottom right | KPI Cards | WoW Change · MoM Change |

### Design Tips
- Use **dark theme** (View → Themes → Executive)
- Add **conditional formatting** on Occupancy: Red < 0.75, Amber 0.75–0.85, Green > 0.85
- Add a **Target reference line** at 85% on trend charts

---

## PROJECT 2 — Service Level Goals Tracking Report
**CSV:** `02_service_level.csv`

### Data Model
- Fact table: `ServiceLevel`
- Dimension: `Dates`
- Calculated: `SVL %`, `SVL Gap`, `SVL Status`

### Pages & Visuals

#### Page 1 — SVL Overview
| Position | Visual | Fields |
|----------|--------|--------|
| Top row (4 cards) | KPI Cards | SVL % · Target · Gap · Status |
| Middle left | Line Chart | SVL % by Date with 85% target line |
| Middle right | Bar Chart | SVL % by LOB |
| Bottom | Gauge | SVL % vs Target |

#### Page 2 — Daily / Interval Analysis
| Position | Visual | Fields |
|----------|--------|--------|
| Top | Slicer | LOB · Date |
| Left | Heat Map (Matrix) | Interval × Day → SVL % |
| Right | Column Chart | Offered vs Answered by Interval |
| Bottom | Table | Date · LOB · SVL% · Gap · Status |

#### Page 3 — Trend & Comparison
| Position | Visual | Fields |
|----------|--------|--------|
| Top | Line Chart | SVL % DoD / WoW / MoM |
| Bottom left | Bar Chart | Abandon Rate by LOB |
| Bottom right | KPI Cards | MoM Change · WoW Change |

### Design Tips
- Use **SVL Color** measure for conditional color on bars (Green/Amber/Red)
- Add **data labels** showing % values on bar charts
- Pin Page 1 as default landing page

---

## PROJECT 3 — Staffing Compliance & Quality Checks
**CSV:** `03_staffing_compliance.csv`

### Data Model
- Fact table: `Staffing`
- Dimension: `Dates`
- Calculated columns: `HC Variance`, `Compliance Flag`

### Pages & Visuals

#### Page 1 — Compliance Overview
| Position | Visual | Fields |
|----------|--------|--------|
| Top row (4 cards) | KPI Cards | Compliance Rate · Avg Adherence · HC Variance · Avg Quality |
| Middle left | Clustered Bar | Rostered vs Actual HC by LOB |
| Middle right | Donut Chart | Compliant vs Understaffed shifts |
| Bottom | Table | Date · LOB · Shift · HC Variance · Flag |

#### Page 2 — Shift Analysis
| Position | Visual | Fields |
|----------|--------|--------|
| Top | Slicer | Shift · LOB · Date Range |
| Left | Matrix | LOB × Shift → HC Variance |
| Right | Line Chart | Compliance Rate by Week |
| Bottom | Bar Chart | Non-Compliance count by LOB |

#### Page 3 — Quality Dashboard
| Position | Visual | Fields |
|----------|--------|--------|
| Top | KPI Card | Avg Quality Score · Rating |
| Middle | Bar Chart | Quality Score by LOB |
| Bottom | Line Chart | Quality Score Trend by Week |

### Design Tips
- Flag understaffed rows in the table using **conditional formatting** (red background)
- Use **Compliance Flag** as a legend color on bar charts
- Add a **target line** at 90% on Compliance Rate trend

---

## PROJECT 4 — Mortgage Market & Portfolio Comparison
**CSV:** `04_mortgage_market.csv`

### Data Model
- Fact table: `Mortgage`
- Dimension: `Dates` (from Month column)
- Dimension: `DimProduct` (distinct Product values)
- Dimension: `DimSegment` (distinct Segment values)

### Pages & Visuals

#### Page 1 — Market vs Client Overview
| Position | Visual | Fields |
|----------|--------|--------|
| Top row (4 cards) | KPI Cards | Market Share % · Rate Differential · Client Volume · Market Volume |
| Middle left | Line Chart | Market vs Client Volume by Month |
| Middle right | Bar Chart | Market Share % by Product |
| Bottom | Table | Segment · Market Vol · Client Vol · Share % |

#### Page 2 — Rate Analysis
| Position | Visual | Fields |
|----------|--------|--------|
| Top | Slicer | Product · Segment · Year |
| Left | Line Chart | Market Rate vs Client Rate by Month |
| Right | Bar Chart | Rate Differential by Product |
| Bottom | KPI Cards | Rate Differential · Label |

#### Page 3 — Risk Dashboard
| Position | Visual | Fields |
|----------|--------|--------|
| Top row (3 cards) | KPI Cards | Market Default Rate · Client Default Rate · Risk Gap |
| Middle | Line Chart | Default Rate Trend: Market vs Client |
| Bottom | Matrix | Product × Segment → Default Rate |

#### Page 4 — YoY Growth
| Position | Visual | Fields |
|----------|--------|--------|
| Top | KPI Cards | Client Vol YoY · Market Vol YoY |
| Middle | Bar Chart | YoY Growth by Product (Client vs Market) |
| Bottom | Line Chart | Cumulative Volume by Year |

### Design Tips
- Use **dual-axis line chart** for Market vs Client Rate comparison
- Color Market series in **blue**, Client series in **orange**
- Add **Risk Flag** conditional formatting in matrix

---

## PROJECT 5 — Retail Sales & Profit Analytics
**CSV:** `05_retail_sales.csv`

### Data Model
- Fact table: `Sales`
- Dimension: `Dates`
- Dimension: `DimProduct` (Category + SubCategory)
- Dimension: `DimRegion` (Region + State)
- Dimension: `DimSegment` (Segment)
- **RLS Role:** Region Manager → `[Region] = USERPRINCIPALNAME()`

### Pages & Visuals

#### Page 1 — Executive Summary
| Position | Visual | Fields |
|----------|--------|--------|
| Top row (4 cards) | KPI Cards | Total Sales · Total Profit · Profit Margin % · YoY Sales ▲▼ |
| Middle left | Line Chart | Sales & Profit by Month (dual axis) |
| Middle right | Map / Filled Map | Sales by State |
| Bottom | Bar Chart | Sales by Category |

#### Page 2 — Category & Product Analysis
| Position | Visual | Fields |
|----------|--------|--------|
| Top | Slicer | Category · Segment · Year |
| Left | Treemap | Sales by Category > SubCategory |
| Right | Bar Chart | Top 10 SubCategory by Profit |
| Bottom | Matrix | SubCategory × Segment → Profit Margin % |

#### Page 3 — Regional Performance
| Position | Visual | Fields |
|----------|--------|--------|
| Top | Slicer | Region · Year |
| Left | Bar Chart | Sales by Region |
| Right | Scatter Plot | Sales vs Profit by State |
| Bottom | Table | State · Sales · Profit · Margin% · YoY% |

#### Page 4 — Discount Impact Analysis
| Position | Visual | Fields |
|----------|--------|--------|
| Top row (3 cards) | KPI Cards | Avg Discount · Discount Impact · Sensitivity |
| Middle | Scatter Plot | Discount % vs Profit Margin % (by SubCategory) |
| Bottom left | Bar Chart | Avg Discount by Category |
| Bottom right | Line Chart | Profit Margin Trend with Discount overlay |

### Design Tips
- Apply **RLS** via Modelling → Manage Roles before publishing
- Use **YoY Sales Arrow** measure as subtitle in Sales KPI card
- Add **Top 5 Flag** slicer to filter product rankings
- Enable **drill-through** from Page 1 Category bar → Page 2

---

## GENERAL POWER BI TIPS

### Publishing to Power BI Service
1. File → Publish → Select your Workspace
2. Set **Scheduled Refresh** (if using import mode)
3. Share the report link with stakeholders

### Saving as Template (.pbit)
1. File → Export → Power BI Template
2. Add description: "WFM Analytics Portfolio — [Project Name]"
3. Upload the `.pbit` to GitHub `pbix/` folder

### Navigation Buttons
- Insert → Buttons → Blank → set Action = Page Navigation
- Add to every page for consistent UX

### Bookmarks (for toggle views)
- View → Bookmarks Panel
- Create bookmarks for "Show Table" / "Show Chart" toggle
