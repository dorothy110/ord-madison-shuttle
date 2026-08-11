# Business Rules

All currency values are USD unless explicitly stated.

## 1. Vehicle economics

| Vehicle | Vendor compensation | Initial customer vehicle total | Initial markup |
|---|---:|---:|---:|
| 7-seat vehicle | $300 | $300 | $0 |
| 15-seat Mercedes | $600 | $650 | $50 |

Rules:

- Vendor compensation and customer vehicle total are separate fields.
- Markup must be configurable by vehicle type and effective date.
- The 7-seat customer total starts at market price with no markup.
- A vehicle plan is commercially covered when confirmed customer revenue is at least its customer vehicle total.
- A departure cannot be presented as final until a specific vehicle plan is locked.
- Extra airport, waiting, toll, gratuity or cancellation charges are not assumed; they require explicit confirmation before inclusion.

## 2. Luggage classification

- Checked suitcase = **1 large item**.
- Carry-on suitcase = **1 small item**.
- Backpack, handbag and ordinary personal item = **not counted**.
- Unusual items such as skis, boxes, instruments, wheelchairs and oversized luggage require manual review.

## 3. Initial capacity matrix

| Vehicle | Passenger load | Large capacity | Small capacity | Status |
|---|---:|---:|---:|---|
| 7-seat | 1–4 | up to 7 | not yet specified | usable rule |
| 7-seat | 5–6 | up to 3 | up to 4 | usable rule |
| 15-seat Mercedes | 10–11 | 20–25 | not yet specified | range; vendor confirmation needed |

Interpretation:

- A “7-seat” label is a vehicle category, not permission to sell seven passenger seats.
- Initial sellable passenger capacity for the 7-seat category is **6**.
- For five or six passengers, both limits apply: no more than three large and no more than four small.
- The source information does not define a complete 7-seat mixed-luggage rule for one to four passengers.
- Initial sellable passenger capacity for the 15-seat Mercedes is **11**.
- The 15-seat large-luggage value is a range. Use **20 as the safe automatic limit** until the vendor confirms the exact vehicle; 21–25 requires manual vendor confirmation.
- Small luggage capacity for the 15-seat vehicle remains unresolved.
- The system must never infer that unlisted small luggage is unlimited.

## 4. Estimated airport-ready time

Use the student's scheduled flight arrival as the starting point.

- Morning arrival: add an estimated **1–2 hours** for deplaning, immigration/customs and baggage.
- Afternoon/evening arrival: add an estimated **2–3 hours**.
- These are planning ranges, not guarantees.
- The user should see the range and be told that immigration, baggage and delays can change it.
- The morning/afternoon cutoff is not yet confirmed and must be configurable.

Suggested fields:

- earliest airport-ready time;
- conservative airport-ready time;
- suggested departure;
- reason for suggestion;
- manual override and override reason.

## 5. Fixed departure schedule and airline affinity

| Departure | Initial airline affinity |
|---|---|
| 8:00 AM | United Airlines |
| 10:00 AM | Japan Airlines |
| 11:30 AM | Korean Air / Turkish Airlines |
| 1:00 PM | Lufthansa |
| 5:30 PM | Cathay Pacific |
| 6:30 PM | Lufthansa / Air France / KLM / Emirates |
| 9:30 PM | Alaska Airlines / Air Canada |
| 10:30 PM | EVA Air |

Rules:

- Airline affinity is a recommendation, not an automatic guarantee.
- The system first checks airport-ready-time feasibility, then airline affinity.
- A suggested departure must not be earlier than the conservative airport-ready time unless operations explicitly overrides it.
- Students must be allowed to indicate the latest departure they will accept.
- Airline names, aliases, seasonal schedules and fixed departure times must be editable by operations.
- Codeshares require manual review in MVP unless the marketing carrier maps cleanly to a configured affinity.

## 6. Customer estimate

The student sees two numbers:

1. **Current estimated booking total**
2. **Estimated per-person amount**

Initial calculation:

- Choose the cheapest currently feasible vehicle plan.
- Use the customer vehicle total: $300 per 7-seat vehicle and $650 per 15-seat vehicle.
- Divide the plan total equally across confirmed billable seats in the pool.
- Multiply the per-seat amount by the booking's party size.
- Round displayed amounts upward to whole dollars.
- Mark the result as an estimate with an expiry time.

Before enough students have confirmed:

- show a range based on current demand and the next feasible pooling scenario;
- show a clearly labeled “maximum currently expected booking total”;
- do not promise a lower final price solely because unconfirmed students are in the pool.

Commercial guardrail:

- Single travelers and single-family bookings generally indicate a maximum acceptable budget of **$200–$300**.
- If the estimate exceeds the submitted budget, do not silently assign the booking; show “needs more passengers or another departure”.
- A competitor benchmark of **RMB 1,800 for a family of three** is recorded for research. Whether this includes the whole vehicle, per-family service, tips, tolls or a specific date is unresolved; it must not yet drive automatic pricing.

## 7. Departure readiness

A pool may move to “ready to lock” only when:

- confirmed revenue covers the customer vehicle total;
- passenger and luggage rules are satisfied;
- vendor availability is confirmed or operations explicitly accepts the risk;
- every confirmed booking has accepted its current price version;
- required contact and flight information is complete;
- no unresolved hard-constraint alert remains.

Cost coverage alone is sufficient for the vendor's commercial willingness to depart, but the product still enforces safety, capacity and confirmation requirements.

## 8. Required audit history

Record:

- demand edits;
- pool moves;
- capacity warnings;
- vehicle-plan changes;
- price versions;
- student acceptance;
- operations overrides;
- vendor acceptance or rejection;
- actual vehicle and driver assignment.

## Open decisions to validate with the vendor

1. Exact make/model represented by each vehicle category.
2. Whether driver occupies one of the stated seats.
3. Complete mixed large/small capacity rules for one to four passengers.
4. Small-luggage capacity for the 15-seat Mercedes.
5. When 21–25 large suitcases are acceptable.
6. Morning versus afternoon cutoff time.
7. Whether fixed times are pickup, passenger meeting or wheels-moving times.
8. Waiting, toll, airport, gratuity and cancellation policies.
9. Airline codeshare and seasonal schedule behavior.
