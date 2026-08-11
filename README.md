# ORD → Madison Student Shuttle

A three-sided shuttle pooling product for:

- **Students and families:** submit flight and luggage details, see an estimated price, and confirm a departure.
- **Operations:** pool demand into fixed departures, select vehicle combinations, verify cost and margin, and lock the plan.
- **Vendors:** receive a clear dispatch request, confirm the vehicle and price, and provide driver details.

## Current product baseline

- Route: Chicago O'Hare International Airport (ORD) → Madison, Wisconsin.
- Vendor cost: 7-seat vehicle **$300**; 15-seat Mercedes **$600**.
- Initial customer-facing vehicle price: 7-seat **$300**; 15-seat **$650**.
- A departure becomes commercially viable once confirmed customer revenue covers the selected vehicle plan.
- Students see an **estimate**, not a final guaranteed price, until the departure and vehicle assignment are locked.
- Checked luggage counts as **large**; carry-on suitcase counts as **small**; backpacks and personal items do not count.
- Fixed departure slots are seeded from the current vendor schedule.

## Core workflow

`Demand → Departure Pool → Vehicle Assignment → Price Estimate → Confirmation → Vendor Dispatch`

## Documentation

- [Product baseline](docs/product-baseline.md)
- [Business rules](docs/business-rules.md)
- [MVP roadmap](docs/mvp-roadmap.md)

## Status

Product definition phase. No application code has been selected or generated yet.


## Access model

The product has three separate entry points:

- **Public student site:** no staff login; students can submit travel demand and view only their own estimate/confirmation flow.
- **Protected operations portal:** separate URL and mandatory account/password authentication for internal staff.
- **Protected vendor portal:** separate URL and mandatory account/password authentication for approved vehicle vendors.

Operations and vendor pages are never exposed through a public role switcher. Vendor users cannot see customer pricing, company margin, other vendors, or unrelated dispatches.

See [Access control and page boundaries](docs/access-control.md).
