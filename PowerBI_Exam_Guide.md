# Power BI Practical Examination Guide

## Complete Exam-Sheet Coverage

| Exam topic | Section in this guide |
|---|---|
| Data import | Section 1 |
| Data preparation | Sections 2 and 3 |
| KPI cards | Sections 6 and 10 |
| Sum, count and average calculations | Sections 6 and 7 |
| Bar and column charts | Section 10 |
| Pie and donut charts | Section 10 |
| Line charts | Section 10 |
| Category-wise comparisons | Sections 7 and 10 |
| Date-wise and month-wise analysis | Section 8 |
| Slicers and interactive filtering | Sections 10 and 11 |
| Multiple report pages | Section 11 |
| Data labels and titles | Section 11 |
| Conditional formatting | Section 11 |
| Dashboard layout and formatting | Section 11 |

Use this file to match an exam question to a Power BI menu, Power Query command or DAX formula.

## 1. Import Data

| Question | Exact action or command | Purpose |
|---|---|---|
| Load CSV | **Home → Get data → Text/CSV** | Import one CSV file. |
| Load all three files | Repeat Text/CSV for Movies, Shows and Bookings | Add all tables to the model. |
| Open Power Query | **Transform Data** | Clean and transform imported tables. |
| Refresh data | **Home → Refresh** | Reload changed CSV files. |
| Copy data from Excel | **Get data → Excel Workbook** | Import Excel source. |
| Combine files | **Combine & Transform** | Merge multiple files with similar structure. |
| View model | **Model view** | Create relationships and inspect tables. |
| View report | **Report view** | Create charts and dashboard visuals. |
| View table values | **Data view** | Inspect loaded table rows. |

## 2. Power Query Data Cleaning

| Question | Power Query action/formula | Purpose |
|---|---|---|
| Remove leading/trailing spaces | Select text column → **Transform → Format → Trim** | Clean accidental spaces. |
| Remove extra non-printing characters | **Transform → Format → Clean** | Remove hidden characters. |
| Convert to uppercase | **Transform → Format → Uppercase** | Standardize text. |
| Convert to lowercase | **Transform → Format → Lowercase** | Standardize text. |
| Convert to title case | **Transform → Format → Capitalize Each Word** | Capitalize words. |
| Replace text | **Transform → Replace Values** | Replace spelling or category values. |
| Replace null | Right-click column → **Replace Values** or **Replace Errors** | Fill or identify missing values. |
| Remove nulls | **Remove Rows → Remove Blank Rows** | Remove rows containing no values. |
| Remove duplicates | **Remove Rows → Remove Duplicates** | Keep first unique row. |
| Remove duplicate ID | Select `Booking_ID` → **Remove Duplicates** | Keep one record per ID. |
| Change type | **Transform → Data Type** | Set text, number or date type. |
| Convert date | **Data Type → Using Locale → Date** | Parse date values correctly. |
| Convert integer | **Data Type → Whole Number** | Use for age, tickets or seats. |
| Convert decimal | **Data Type → Decimal Number** | Use for ratings, amounts or prices. |
| Keep valid rows | **Home → Remove Rows → Keep Rows** or **Number Filters** | Filter valid ranges. |
| Remove invalid negative values | **Number Filters → Does Not Equal Less Than** / custom filter | Remove negative durations, prices or tickets. |
| Split text | **Transform → Split Column → By Delimiter** | Split names or combined fields. |
| Extract characters | **Add Column → Extract** | Extract first/last/text values. |
| Add custom column | **Add Column → Custom Column** | Create a calculated column using Power Query M. |
| Merge two queries | **Home → Merge Queries** | Join tables using matching columns. |
| Append tables | **Home → Append Queries** | Stack tables with same columns. |

## 3. Power Query M Formulas

| Question | Formula | Purpose |
|---|---|---|
| Trim text | `Table.TransformColumns(table, {{"Genre", Text.Trim, type text}})` | Remove spaces. |
| Parse date | `Table.TransformColumns(table, {{"Booking_Date", each try Date.From(_) otherwise null}})` | Convert dates safely. |
| Replace text | `Table.ReplaceValue(table, "Canceled", "Cancelled", Replacer.ReplaceText, {"Booking_Status"})` | Standardize status. |
| Fill null with fixed text | `Table.ReplaceValue(table, null, "Unknown", Replacer.ReplaceValue, {"Genre"})` | Replace missing text. |
| Filter age | `Table.SelectRows(table, each [Customer_Age] >= 5 and [Customer_Age] <= 100)` | Remove invalid ages. |
| Add age group | `if [Customer_Age] <= 18 then "Child/Teen" else if [Customer_Age] <= 30 then "18-30" else "31-45"` | Make age categories. |
| Add month name | `Date.MonthName([Booking_Date])` | Create month name. |
| Add year | `Date.Year([Booking_Date])` | Create year. |
| Add weekday | `Date.DayOfWeekName([Booking_Date])` | Create weekday name. |

## 4. Relationships

| Question | Action/formula | Purpose |
|---|---|---|
| Open relationship manager | **Model view** | View existing links. |
| Create Movies-Shows link | Drag `Movies[Movie_ID]` to `Shows[Movie_ID]` | Link movie details to shows. |
| Create Shows-Bookings link | Drag `Shows[Show_ID]` to `Bookings[Show_ID]` | Link shows to booking transactions. |
| Set cardinality | **One-to-many** | One movie/show can have many shows/bookings. |
| Set direction | **Single** | Filters flow from Movies/Shows to Bookings. |
| Enforce relationship | **Make this relationship active** | Use the link in reports. |
| Identify fact table | Select `Bookings` | Transaction table containing numeric measures. |
| Identify dimension tables | Select `Movies` and `Shows` | Tables containing descriptive attributes. |
| Create inactive relationship | Turn relationship off | Use only with `USERELATIONSHIP` in DAX. |

## 5. Calculated Columns

### Age Group

```DAX
Age Group =
SWITCH(
    TRUE(),
    Bookings[Customer_Age] <= 18, "Child/Teen",
    Bookings[Customer_Age] <= 30, "18-30",
    Bookings[Customer_Age] <= 45, "31-45",
    Bookings[Customer_Age] <= 60, "46-60",
    "60+"
)
```

Use: creates category by customer age.

### Booking Year

```DAX
Booking Year = YEAR(Bookings[Booking_Date])
```

Use: extracts year.

### Booking Month

```DAX
Booking Month =
FORMAT(Bookings[Booking_Date], "MMM")
```

Use: creates month abbreviation.

### Booking Month Number

```DAX
Booking Month Number = MONTH(Bookings[Booking_Date])
```

Use: sorts month names in chronological order.

### Gross Ticket Amount

```DAX
Gross Ticket Amount =
Bookings[Tickets] * RELATED(Shows[Ticket_Price])
```

Use: calculates ticket value from show price.

### Amount Match

```DAX
Amount Match =
IF(
    Bookings[Ticket_Amount]
        - Bookings[Discount]
        + Bookings[Convenience_Fee]
        = Bookings[Final_Amount],
    "Match",
    "Mismatch"
)
```

Use: checks whether stored final amount is correct.

## 6. Basic Measures

### Total Revenue

```DAX
Total Revenue = SUM(Bookings[Final_Amount])
```

Use: sum of booking revenue.

### Ticket Revenue

```DAX
Ticket Revenue = SUM(Bookings[Ticket_Amount])
```

Use: sum before discounts and fees.

### Total Tickets

```DAX
Total Tickets = SUM(Bookings[Tickets])
```

Use: count of tickets.

### Booking Count

```DAX
Booking Count = COUNTROWS(Bookings)
```

Use: number of booking records.

### Customer Count

```DAX
Customer Count = DISTINCTCOUNT(Bookings[Customer_ID])
```

Use: number of unique customers.

### Show Count

```DAX
Show Count = DISTINCTCOUNT(Shows[Show_ID])
```

Use: number of distinct shows.

### Movie Count

```DAX
Movie Count = DISTINCTCOUNT(Movies[Movie_ID])
```

Use: number of distinct movies.

### Average Rating

```DAX
Average Rating = AVERAGE(Movies[Rating])
```

Use: average movie rating.

### Average Ticket Price

```DAX
Average Ticket Price = AVERAGE(Shows[Ticket_Price])
```

Use: average show ticket price.

### Average Final Amount

```DAX
Average Final Amount = AVERAGE(Bookings[Final_Amount])
```

Use: average booking spend.

### Maximum/Minimum Revenue

```DAX
Maximum Revenue = MAX(Bookings[Final_Amount])
Minimum Revenue = MIN(Bookings[Final_Amount])
```

Use: find highest/lowest booking amount.

## 7. Filtered and Percentage Measures

### Confirmed Bookings

```DAX
Confirmed Bookings =
CALCULATE(
    [Booking Count],
    Bookings[Booking_Status] = "Confirmed"
)
```

Use: count Confirmed status only.

### Cancelled Bookings

```DAX
Cancelled Bookings =
CALCULATE(
    [Booking Count],
    Bookings[Booking_Status] = "Cancelled"
)
```

Use: count Cancelled status only.

### Confirmation Rate

```DAX
Confirmation Rate % =
DIVIDE([Confirmed Bookings], [Booking Count], 0)
```

Use: confirmed percentage of all bookings.

### Cancellation Rate

```DAX
Cancellation Rate % =
DIVIDE([Cancelled Bookings], [Booking Count], 0)
```

Use: cancelled percentage of all bookings.

### Confirmed Revenue

```DAX
Confirmed Revenue =
CALCULATE(
    [Total Revenue],
    Bookings[Booking_Status] = "Confirmed"
)
```

Use: revenue only for Confirmed bookings.

### Revenue by Genre

```DAX
Revenue by Genre =
CALCULATE(
    [Total Revenue],
    REMOVEFILTERS(Shows),
    KEEPFILTERS(Movies)
)
```

Use: total is controlled by Movie dimension filters.

In normal visuals, simply place Genre in Axis and `Total Revenue` in Values. The special CALCULATE expression is mainly useful for formula-based filtering questions.

## 8. DAX Time Intelligence

| Question | DAX formula | Purpose |
|---|---|---|
| Create date table | `Dates = CALENDAR(MIN(Bookings[Booking_Date]), MAX(Bookings[Booking_Date]))` | Create date calendar. |
| Year in date table | `Year = YEAR(Dates[Date])` | Add year. |
| Month number | `Month Number = MONTH(Dates[Date])` | Sort month. |
| Month name | `Month = FORMAT(Dates[Date],"MMMM")` | Show month name. |
| Month-to-date | `MTD Revenue = TOTALMTD([Total Revenue], Dates[Date])` | Revenue from month start. |
| Year-to-date | `YTD Revenue = TOTALYTD([Total Revenue], Dates[Date])` | Revenue from year start. |
| Previous month | `Previous Month Revenue = CALCULATE([Total Revenue], DATEADD(Dates[Date],-1,MONTH))` | Previous-month comparison. |
| Year growth | `YoY Revenue = [Total Revenue] - CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Dates[Date]))` | Revenue difference versus last year. |

Before time intelligence, mark `Dates` as a date table and relate `Dates[Date]` to `Bookings[Booking_Date]`.

## 9. Ranking and Top N

### Rank Movies by Revenue

```DAX
Movie Revenue Rank =
RANKX(
    ALL(Movies[Movie_Name]),
    [Total Revenue],
    ,
    DESC,
    DENSE
)
```

Use: ranks every movie even with selected genres.

### Top 10 Revenue

```DAX
Top 10 Revenue =
VAR TopMovies =
    TOPN(10, ALL(Movies[Movie_Name]), [Total Revenue], DESC)
RETURN
    CALCULATE([Total Revenue], KEEPFILTERS(TopMovies))
```

Use: returns revenue for ten highest-revenue movies.

### Top Movie Name

```DAX
Top Movie =
TOPN(1, SUMMARIZE(Movies, Movies[Movie_Name], "Revenue", [Total Revenue]), [Revenue], DESC)
```

Use: table-based top movie calculation. For a simple dashboard, sort a bar chart descending and enable a Top 10 filter.

## 10. Visuals and Where to Use Them

| Practical question | Recommended visual |
|---|---|
| Compare revenue by genre | Clustered bar chart |
| Compare booking status | Pie or donut chart |
| Show monthly trend | Line chart |
| Show revenue and tickets together | Combo chart |
| Show top cities | Clustered column chart or treemap |
| Compare age groups | Column chart |
| Show rating vs budget relationship | Scatter chart |
| Show multiple measures by city/genre | Matrix |
| Show one overall value | Card or KPI |
| Filter by City/Genre/Status | Slicer |
| Show ranking table | Table or Matrix |
| Show revenue by movie and genre | Decomposition tree |

### Common Visual Field Setup

- **Clustered bar chart:** Axis = `Genre`; Values = `Total Revenue`.
- **Pie chart:** Legend = `Booking_Status`; Values = `Booking Count`.
- **Line chart:** Axis = `Booking Month Number` or Date; Values = `Total Revenue`.
- **Scatter:** X = `Production_Budget`; Y = `Rating`; Details = `Movie_Name`.
- **Matrix:** Rows = `City`; Columns = `Payment_Mode`; Values = `Total Revenue`.
- **Table:** Columns = `Movie_Name`, `Genre`, `Average Rating`, `Total Revenue`.
- **Slicer:** Field = `City`, `Genre`, `Show_Type`, `Payment_Mode`, `Booking_Status`.

## 11. Dashboard Formatting

| Question | Action |
|---|---|
| Format amount | Select measure → **Column tools/Measure tools → Formatting → Currency** |
| Format percentage | Select measure → **Measure tools → Percentage** |
| Add conditional formatting | Select visual → **Format visual → Cell elements/Bar color** |
| Add data labels | **Format visual → Data labels → On** |
| Sort a chart | Click three dots → **Sort axis** → choose field and ascending/descending |
| Add title | **Format visual → Title → On** |
| Change page size | **Format page → Canvas settings** |
| Improve visual interaction | **Format → Edit interactions** |
| Sync slicers | **View → Sync slicers** |
| Clear filters | **Clear filters** icon or slicer select-all |
| Export report data | Visual three dots → **Export data** |

## 12. Quick Viva Answers

- **DAX:** Formula language used for calculated columns and measures.
- **Measure:** Dynamic calculation based on current filters.
- **Calculated column:** Row-level value calculated for the table.
- **Fact table:** Contains bookings and numeric business transactions.
- **Dimension table:** Describes business entities such as movies and shows.
- **Relationship:** Link between tables using common keys.
- **Filter context:** Active visual/slicer/page/report filters.
- **Row context:** Current row in a calculated column or iterator.
- **CALCULATE:** Changes filter context for an expression.
- **DIVIDE:** Performs division and handles divide-by-zero.
- **SUM:** Totals values.
- **COUNTROWS:** Counts rows in a table.
- **DISTINCTCOUNT:** Counts unique values.
- **RELATED:** Returns a related value from the one-side table.
- **RANKX:** Assigns rank values.
- **TOPN:** Returns the top N rows from a table.
