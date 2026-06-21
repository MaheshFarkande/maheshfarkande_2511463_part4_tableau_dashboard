# Tableau Calculated Fields: `dashboard_sales_data`

Reference for the five calculated fields.


## 1. Profit Margin

```
SUM([Profit]) / SUM([Sales])
```

Written as an aggregate ratio (rather than `[Profit] / [Sales]`) so it stays correct at any level of detail — region, category, or the whole view.
## 2. Cost

```
[Sales] - [Profit]
```

A row-level calculation — Cost is implied, since the dataset has no explicit cost column. Need to Aggregate it in views with `SUM([Cost])`.

## 3. Average Order Value

```
SUM([Sales]) / COUNTD([Customer Id])
```

## 4. Return Rate

```
SUM([Return Flag]) / COUNTD([Order Id])
```

Works because `return_flag` is stored as 1/0, so summing it counts returned orders. Need to Format as **Percentage**. (Returns are ≈ 4.5% overall.) 

## 5. Shipping Delay Bucket

```
IF [Delivery Days] = 0 THEN "Same Day"
ELSEIF [Delivery Days] <= 2 THEN "1-2 days (Fast)"
ELSEIF [Delivery Days] <= 4 THEN "3-4 days (Standard)"
ELSEIF [Delivery Days] <= 6 THEN "5-6 days (Slow)"
ELSE "7+ days (Delayed)"
END
```

Buckets span the actual data range (0–9 days). This produces a **dimension** — to  drag it to Rows/Color to group orders. 
---

