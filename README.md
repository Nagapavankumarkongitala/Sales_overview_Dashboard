# Sales_overview_Dashboard

## Overview of the Dashboard

The **Sales Overview Dashboard** provides a comprehensive view of sales performance across different regions, including Central, East, South, and West. The dashboard is divided into several sections, offering insights into various metrics such as sales, profit, and quantity.

### Sales by Region
- **Central** : Current Year (CY) Sales of $147.10K with Previous Year (PY) Sales of $147.43K.
- **East** : CY Sales of $213.82K with PY Sales of $181.34K.
- **South** : CY Sales of $122.91K with PY Sales of $93.61K.
- **West** : CY Sales of $250.13K with PY Sales of $187.48K.

### Sales by State
- **Map of the United States** : Sales data is represented by colored circles.
- **Bar Chart** : Displays sales figures for the top states, with California leading at approximately $0.1M.

### Current Year (CY) & Previous Year (PY) Metrics for 2024
A table summarizing the metrics for each region:

| Region  | CY Sales      | PY Sales      | YoY Sales | CY Profit     | PY Profit     | YoY Profit | CY Qty | PY Qty | YoY Qty |
|---------|---------------|---------------|-----------|---------------|---------------|------------|--------|--------|---------|
| West    | $250,128.37   | $187,480.18   | 33%       | $43,808.96    | $24,051.61    | 82%        | 4270   | 3025   | 41%     |
| East    | $213,816.00   | $181,341.54   | 18%       | $33,375.13    | $20,263.54    | 65%        | 3450   | 2827   | 20%     |
| Central | $147,098.13   | $147,429.38   | -0%       | $17,550.84    | $19,899.16    | -62%       | 2880   | 2359   | 22%     |
| South   | $122,905.86   | $93,610.22    | 31%       | $8,848.91     | $17,702.81    | -50%       | 1915   | 1614   | 19%     |
| **Total**| **$733,948.35** | **$609,861.32** | **20%** | **$93,583.84** | **$81,917.12** | **14%**    | **12515** | **9870** | **27%** |

## DAX Formulas Used in the Dashboard

### CY Sales
```DAX
Cy Sales = 
VAR SelectedYear = SELECTEDVALUE('Calender Table'[Year])
VAR CurrentYearSales = CALCULATE([Total sales], 'Calender Table'[Year] = SelectedYear)
RETURN CurrentYearSales
```
- **Necessity**: This formula calculates the total sales amount for the current year.

### PY Sales
```DAX
Py Sales = 
VAR SelectedYear = SELECTEDVALUE('Calender Table'[Year])
VAR PreviousYearSales = CALCULATE([Total sales], 'Calender Table'[Year] = SelectedYear -1)
RETURN 
        IF(ISBLANK(PreviousYearSales), "0",PreviousYearSales)

```
- **Necessity**: This formula calculates the total sales amount for the previous year, allowing for year-over-year comparisons.

### YoY Sales
```DAX
YoY Sales = 
    VAR Currentyearsales = [CY Sales]
    VAR Previousyearsales = [PY Sales]
    VAR YoYChange = 
        IF(
            ISBLANK(Previousyearsales) || Previousyearsales = 0, 
            0, 
            DIVIDE(Currentyearsales - Previousyearsales, Previousyearsales)
        )
    VAR PRINT = 
        IF(
            YoYChange = 0, 
            "0%",
            IF(
                YoYChange > 0, 
                UNICHAR(9650) & " " & FORMAT(YoYChange, "0%"),
                UNICHAR(9660) & " " & FORMAT(YoYChange, "0%")
            )
        )
    RETURN 
        PRINT
```
- **Necessity** : This formula calculates the year-over-year sales growth percentage, providing insight into sales performance trends.

### CY Profit
```DAX
Cy Profit = 
VAR SelectedYear = SELECTEDVALUE('Calender Table'[Year])
VAR CurrentYearProfit = CALCULATE([Total profit], 'Calender Table'[Year] = SelectedYear)
RETURN CurrentYearProfit
```
- **Necessity** : This formula calculates the total profit amount for the current year.

### PY Profit
```DAX
Py Profit = 
VAR SelectedYear = SELECTEDVALUE('Calender Table'[Year])
VAR PreviousYearProfit = CALCULATE([Total profit], 'Calender Table'[Year] = SelectedYear - 1)
RETURN
        IF(ISBLANK(PreviousYearProfit), "0", PreviousYearProfit)
```
- **Necessity** : This formula calculates the total profit amount for the previous year.

### YoY Profit
```DAX
YoY Profit = 
    VAR Currentyearprofit = [CY Profit]
    VAR Previousyearprofit = [PY Profit]
    VAR YoYChange = 
        IF(
            ISBLANK(Previousyearprofit) || Previousyearprofit = 0, 
            0, 
            DIVIDE(Currentyearprofit - Previousyearprofit, Previousyearprofit)
        )
    VAR PRINT = 
        IF(
            YoYChange = 0, 
            "0%",
            IF(
                YoYChange > 0, 
                UNICHAR(9650) & " " & FORMAT(YoYChange, "0%"),
                UNICHAR(9660) & " " & FORMAT(YoYChange, "0%")
            )
        )
    RETURN 
        PRINT
```
- **Necessity** : This formula calculates the year-over-year profit growth percentage.

### CY Qty
```DAX
Cy Qty = 
VAR SelectedYear = SELECTEDVALUE('Calender Table'[Year])
VAR CurrentYearQty = CALCULATE([Total Quantity], 'Calender Table'[Year] = SelectedYear)
RETURN CurrentYearQty
```
- **Necessity**: This formula calculates the total quantity sold for the current year.

### PY Qty
```DAX
Py Qty = 
VAR SelectedYear = SELECTEDVALUE('Calender Table'[Year])
VAR PreviousYearQty = CALCULATE([Total Quantity], 'Calender Table'[Year] = SelectedYear - 1)
RETURN 
 IF(ISBLANK(PreviousYearQty), "0", PreviousYearQty)
```
- **Necessity**: This formula calculates the total quantity sold for the previous year.

### YoY Qty
```DAX
YoY Qty = 
    VAR Currentyearqty = [CY Qty]
    VAR Previousyearqty = [PY Qty]
    VAR YoYChange = 
        IF(
            ISBLANK(Previousyearqty) || Previousyearqty = 0, 
            0, 
            DIVIDE(Currentyearqty - Previousyearqty, Previousyearqty)
        )
    VAR PRINT = 
        IF(
            YoYChange = 0, 
            "0%",
            IF(
                YoYChange > 0, 
                UNICHAR(9650) & " " & FORMAT(YoYChange, "0%"),
                UNICHAR(9660) & " " & FORMAT(YoYChange, "0%")
            )
        )
    RETURN 
        PRINT
```
- **Necessity** : This formula calculates the year-over-year quantity growth percentage.

![alttext]()
# Conclusion
. The Sales Overview Dashboard is an invaluable tool for analyzing sales performance across different regions and states.

. By leveraging key metrics and DAX formulas, the dashboard provides insights into current and past sales, profit, and quantity.

. The year-over-year comparisons help in understanding trends, identifying growth opportunities, and making informed business decisions.

. The detailed analysis aids in strategizing and enhancing overall sales performance

. These DAX formulas are essential for providing detailed and accurate insights into sales performance, profit trends, and quantity growth over time. They enable year-over-year comparisons and help identify areas of improvement or success.
```

