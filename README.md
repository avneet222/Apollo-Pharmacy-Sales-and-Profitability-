# Apollo Pharmacy Sales and Profitability

An end-to-end Power BI project that diagnoses why an Apollo pharmacy chain is growing revenue but losing gross margin — built as a 3-page executive-to-operational drill path: Sales Overview → Diagnostic Root Cause Analysis → Store & Product Drillthrough.

#Business Problem : 

Leadership sees healthy top-line growth but shrinking profitability, and needs to know where the money is leaking — across discounts, returns, COGS, stores, products, channels, and customer segments.

Metric	2024 → 2025	Read

Realized Revenue	+11.2%	Growth is real

Gross Margin %	32.2% → 27.6%	Margin compressing despite growth

Return Rate	3.15% → 1.69%	Operational improvement, but not enough to offset margin loss

#Dashboard Structure : 

The report is built as a 3-page drill path, from executive summary down to transaction-level detail.

Page 1 — Sales Overview (Revenue & Profitability)

Executive KPI cards (Gross Margin, Realized Revenue, COGS, Discount %, Transactions, Avg Bill Value) with MoM deltas, monthly revenue vs. gross margin % trend, revenue by sales channel, top product categories by revenue and margin, a revenue leakage bridge (Gross Sales → Discount Leakage → Return Leakage → Total), an India state-level revenue map, and a state-wise performance table.

Business questions answered: Are we growing profitably? Which store, category, channel, or campaign drives margin?

<img width="631" height="311" alt="Screenshot 2026-08-12 162620" src="https://github.com/user-attachments/assets/1cea8224-5216-4c55-8022-fffb77a218ac" />

Page 2 — Diagnostic Root Cause Analysis

Discount leakage decomposition tree (channel → payment mode → campaign → state), an hourly revenue heatmap by day of week, margin at risk by therapy area, and a return-reason Pareto chart with cumulative return %.

Business questions answered: Where exactly is margin leaking, and why?

<img width="629" height="310" alt="Screenshot 2026-08-12 162711" src="https://github.com/user-attachments/assets/b2a8d77e-6901-45a2-b5c9-a9c33859d9c0" />

Page 3 — Store & Product Drillthrough

Transaction-level drillthrough table (Transaction ID, date, state, city, channel, category, product, quantity, revenue, margin %, discount %, return %) with in-cell data bars and conditional formatting, filterable from Pages 1 and 2.

Business questions answered: Which specific stores, products, and transactions are driving the numbers above?

<img width="635" height="326" alt="Screenshot 2026-08-12 162745" src="https://github.com/user-attachments/assets/d8557b49-fd98-47f2-99d6-ce8e0d5dfa71" />

#KPI Logic

All measures follow a single revenue waterfall — no black-box DAX, every number traces back to this chain:

Gross Sales
  − Discount        → Net Sales
  − Returns          → Realized Revenue
  − COGS             → Gross Margin
  
Term	Definition	Calculation
Gross Sales	Total sales value before discount, return, or cost impact	Quantity × Unit Selling Price
Discount	Price reduction from campaign, channel offer, loyalty, or store promotion	Gross Sales × Discount %
Net Sales	Billed value after discount, before returns	Gross Sales − Discount Amount
Return Amount	Revenue reversal from cancelled, damaged, expired, or customer-returned products	Returned Qty × Unit Selling Price
Realized Revenue	Final revenue retained after discount and return impact	Net Sales − Return Amount
COGS	Purchase/inventory cost of products sold	Quantity Sold × Unit Cost
Gross Margin	Profit left after product cost	Realized Revenue − COGS
GM %	Profitability ratio	Gross Margin ÷ Realized Revenue
AOV	Average order value	Realized Revenue ÷ Orders
Orders	Distinct customer transactions	Distinct Transaction IDs
Return Rate %	Share of net sales lost to returns	Return Amount ÷ Net Sales
Stockout Rate %	Share of product lines with zero stock	Stockout Lines ÷ Total Lines
Low Stock Rate %	Share of products below reorder level	Low Stock Lines ÷ Total Lines
Avg SLA	Average fulfillment time	Total Fulfillment Minutes ÷ Orders

#Key Insights

Discount leakage (₹220.85K) is the larger of the two margin drains, more than 10x the return leakage (₹18.75K) — the root-cause tree traces most of it to Walk-in and Home Delivery channels paid via UPI and Wallet.

598 transactions carried a "high discount" flag, averaging ₹68.87 discount per transaction — concentrated in specific campaign types rather than spread evenly.

Diabetic Nutrition and Anti-Hypertensive/Anti-Diabetic categories carry the highest margin-at-risk (₹525K, ₹246K, ₹244K respectively), making them the priority for pricing/discount policy review.

"Prescription Changed" and "Damaged Pack" account for ~60% of cumulative return value, pointing to a fulfillment/inventory-quality issue rather than a pricing one.

Revenue peaks sharply on weekend evenings (Fri–Sat, 19:00–21:00), useful for staffing and campaign timing decisions.

Walk-in remains the dominant channel by revenue (₹0.55M), but App and Home Delivery show margin dynamics worth monitoring at the channel level.

#Data Model

Built on a star schema:

Fact table: transaction-level sales (quantity, unit price, discount %, return flag, cost)
Dimension tables: Date, State/City, Store, Product/Category, Sales Channel, Payment Mode, Campaign

Relationships and KPI measures (Realized Revenue, Gross Margin, GM %, discount/return leakage) are built entirely in DAX on top of this model — no pre-aggregated source data.

#Tools & Techniques

Power BI Desktop — data modeling (star schema), DAX measures, Power Query transformations
DAX — waterfall/bridge logic, decomposition tree, Pareto with cumulative %, drillthrough filtering
Visuals used — KPI cards, combo trend chart, bridge/waterfall, decomposition tree, heatmap matrix, Pareto chart, shape map (India states), drillthrough table with data bars
Repository Contents
├── README.md
├── screenshots/
│   ├── 01_sales_overview.png
│   ├── 02_diagnostic_root_cause.png
│   └── 03_store_product_drillthrough.png
├── Apollo_Sales_Dashboard.pbix        (Power BI report file)
└── data/                              (source/synthetic dataset, if shared)
How to Use
Clone the repo and open Apollo_Sales_Dashboard.pbix in Power BI Desktop.
Use the date range slider and State/Category slicers on Page 1 to filter the whole report.
Right-click any row on Page 1 or Page 2 and select Drillthrough → Store & Product Drillthrough to jump to transaction-level detail; use Reset Drill to return.
Hover any KPI card or chart element for tooltips with MoM/context detail.

#About This Project

Built as a portfolio project to demonstrate diagnostic (not just descriptive) BI: starting from a single business question — why is margin dropping while revenue grows — and building the data model, KPI logic, and report navigation to answer it end to end.
