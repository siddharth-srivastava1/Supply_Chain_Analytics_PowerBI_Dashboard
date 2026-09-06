# Supply_Chain_Analytics_PowerBI_Dashboard
# Supply Chain Command Center (Power BI)

## Why this exists

Most supply chain reporting lives in silos — procurement has its spreadsheet, warehousing has its own, sales tracks revenue somewhere else entirely. This report pulls sales, inventory, procurement, production, and shipment data into a single model so that a lead time spike, a revenue dip, and a defect rate climb can all be traced back to the same root cause instead of chased across five different tools.

Six pages, one dataset, no context-switching.

## How it's organized

**Home** is a landing screen — nothing analytical, just a banner and a way into the rest of the report.

**Overview** is the executive view: total revenue, profit, margin, order volume, and shipment counts at a glance, plus a set of navigation tiles that jump straight into whichever functional area needs attention.

From there, four pages go deep on one part of the chain each — **Supplier** (lead times, cost, quality by country and by vendor), **Inventory** (stock position against safety stock and reorder points, tied to defect rates from production), **Shipment** (delivery status, delay causes, carrier performance), and **Customer** (channel mix, discounting behavior, profit trends over time).

## What's under the hood

The data model is a conventional star schema — five fact tables (sales, inventory, procurement, production, shipment) hanging off five shared dimensions (date, product, customer, supplier, facility) through sixteen active relationships. Nothing exotic there.

What's less conventional is how the visuals are styled. Rather than relying on Power BI's native card and KPI visuals, most of the colored panels, navigation tiles, and section banners on this report are actually a single custom visual — **HTML Content** — being fed a DAX measure that returns a complete HTML/CSS string, animations included. A measure like `Total_Revenue` does the arithmetic; a second measure wraps that number in styled markup and hands it to the visual to render. It's a workaround for Power BI's fairly limited native formatting options, and it's why the model has 41 measures instead of the roughly two dozen you'd expect from the KPIs alone — about a third of them exist purely for presentation, not calculation.

## The numbers behind it

Revenue, profit, and margin come straight off `fact_sales`. Inventory health is read off stock level versus safety stock and reorder point. Supplier quality blends lead time, unit cost, and a quality score per order. Shipment reliability is tracked through a "Perfect Order" measure — a shipment counts only if it was delivered *and* came from a batch with a defect rate under 1%, tying production quality directly to delivery performance rather than treating them as separate metrics. Growth is measured period-over-period against the same period a year prior.

## Getting it running yourself

You'll need Power BI Desktop and the free **HTML Content** custom visual (available from within Power BI Desktop — Insert > Get more visuals). Load the ten source CSVs, build the relationships as described above, add one calculated column on the date table (a three-letter month abbreviation used by most of the trend charts), then create the 41 measures and assemble the six pages. A full field-by-field build guide, the exact DAX for every measure, the color theme, and the raw data are packaged separately alongside this file.

## Caveats

The dataset is synthetic — it doesn't represent a real company, and a handful of measures defined in the model (a couple of early-draft banner and KPI-panel variants) aren't actually wired into any visual; they're leftovers from earlier iterations of the design rather than bugs to chase down.

## License

Built for portfolio and learning purposes. Data is synthetic and not derived from any real organization.
