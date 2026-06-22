# Chart Selection Justification — Executive Sales Dashboard

This document explains the chart choices in the Executive Sales Dashboard 
---

## 1. Per-Chart Justification

### Monthly Sales Trend — Line chart
**Business question:** How are sales changing over time?
A line chart is the correct choice for a continuous time series: it makes month-to-month movement, seasonality, and momentum immediately readable, which a bar chart would fragment. Time runs left-to-right along a continuous `Order Date` axis (Feb 2024 → Dec 2025), and each point is labeled so executives can read exact monthly values without hovering. Placing it as the full-width band at the bottom gives the trend the horizontal space it needs and anchors the time context for the whole dashboard.

### Sales by Region — Filled map
**Business question:** Where is revenue concentrated geographically?
Because the geography is real (Indian states), a filled map is the natural encoding — it adds spatial context that a bar chart can't, letting the viewer see regional clusters at a glance. Sales totals are labeled directly on each state so the map communicates exact figures, not just relative shading. This answers "which areas drive revenue" faster than a table.

### Profit by Customer Segment — Highlight table
**Business question:** How does profitability vary across Category × Region × Customer Segment?
This is a three-dimensional comparison (Category and Region on rows, Customer Segment on columns), which is exactly the case a highlight table handles best: color encodes magnitude while the numbers remain exact. A bar chart would struggle with three simultaneous dimensions. The sequential blue gradient lets the eye spot the strongest and weakest cells quickly while preserving precise values for interpretation.

### Profit & Profit-Margin by Category, Sub-Category — Paired horizontal bar charts
**Business question:** Which sub-categories drive profit, and are they efficient?
Two side-by-side bar panels separate **absolute Profit** from **Profit Margin**, which answers two different questions at once: who contributes the most money vs. who is most efficient per dollar. Horizontal bars are used because sub-category labels are text and read more cleanly on the y-axis. The Category → Sub-Category hierarchy is preserved on the row shelf, grouping related items together.

### Ship Mode Performance — Paired horizontal bar charts
**Business question:** Which shipping mode causes delays, and what is the margin impact?
Bars compare **Avg. Delivery Days** and **Profit Margin** across the four ship modes — the right encoding for comparing a measure across a small set of categories. Pairing the two panels ties operational performance (speed) to financial outcome (margin) so the viewer can judge trade-offs, e.g. whether faster modes erode margin.

---
