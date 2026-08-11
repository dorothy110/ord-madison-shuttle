# Product Baseline

## Product decision

Build a lightweight three-sided operations product for fixed ORD → Madison shuttle departures.

This is **not** an Uber-style marketplace and is **not** a general route optimizer. The first version solves:

1. collect student and family demand;
2. estimate airport-ready time;
3. suggest a fixed departure slot;
4. pool compatible demand;
5. choose a commercially viable vehicle plan;
6. show an estimated customer price;
7. lock the trip and dispatch the vendor.

## Users

### Student or family

Provides:

- travel date;
- airline and flight number;
- scheduled arrival time;
- party size;
- checked luggage count;
- carry-on suitcase count;
- phone or preferred contact method;
- latest acceptable departure time;
- special requirements.

Receives:

- suggested fixed departure;
- estimated airport-ready time;
- estimated total price for the booking;
- estimated per-person price;
- estimate status and expiry;
- final confirmation after operations locks the departure.

### Operations

Needs to see:

- all incoming demand;
- departure pools by date and time;
- passengers and luggage;
- recommended vehicle plan;
- vendor cost, customer revenue and gross margin;
- uncovered cost needed to trigger a departure;
- conflicts and missing information;
- student and vendor response text;
- full change history.

### Vendor

Receives only operational information:

- service date;
- pickup and departure time;
- ORD meeting point;
- Madison destination or drop-off plan;
- passenger count;
- large and small luggage totals;
- requested vehicle type and quantity;
- confirmed vendor compensation;
- dispatch status.

The vendor does not see student prices or company margin.

## Commercial principles

- Vendor willingness to depart is based on whether the selected vehicle cost is covered.
- Customer-facing price and vendor compensation must be stored separately.
- Vehicle cost coverage is a necessary departure condition, not the only operational condition.
- Every price is an estimate until the assignment is locked.
- A later vehicle change creates a new price version; it must not silently rewrite an accepted quote.
- Operations can override a recommendation only with a reason recorded in the audit history.

## Core objects

- Demand
- Departure Pool
- Vehicle Type
- Vehicle Assignment
- Price Estimate
- Booking Confirmation
- Vendor Dispatch
- Status Event

## Initial success measures

- percentage of complete demand submissions;
- percentage of demand assigned to a departure;
- average wait after estimated airport-ready time;
- vehicle utilization;
- gross margin per departure;
- percentage of estimates converted to confirmations;
- vendor acceptance rate;
- number of locked-plan changes;
- on-time departure rate.

## Explicitly deferred

- multi-stop home pickup;
- real-time route optimization;
- vendor bidding marketplace;
- live GPS;
- dynamic surge pricing;
- driver mobile app;
- automated refunds;
- automatic flight-data integration.


## Confirmed access boundary

The product has two entry points, not a public three-role switcher.

### Public student site

- Public intake plus a secure, booking-scoped progress page.
- Shows aggregate pool progress without revealing other passenger identities.
- Shows current and projected per-person prices as the pool grows.
- Does not deliver dispatch controls, the full demand list or other bookings to the public client.

### Protected shared dispatch workspace

- One authenticated collaboration page for operations staff and vehicle vendors.
- Both sides can help group students, check capacity and assign vehicles.
- Shows the simple economics needed for collaboration:
  - 7-seat: customer total $300, vendor compensation $300, margin $0.
  - 15-seat: customer total $650, vendor compensation $600, margin $50.
- Individual actions remain tied to named accounts and recorded in audit history.
- Manager-only actions such as lock, reopen and rule override require permission and a reason.

Authentication is enforced by the server. Hiding a page or button in browser code is not access control.

## Student pooling progress

After submission, the student receives a secure progress view containing:

- booking status;
- suggested departure;
- current confirmed passengers in the pool;
- selected or currently likely vehicle plan;
- current estimated per-person price;
- the student's estimated booking total;
- remaining capacity;
- the next useful threshold, such as “再来 2 人，预计人均价降至 $X”;
- a projection table for feasible future passenger counts;
- estimate timestamp and expiry;
- an explanation that unconfirmed passengers are projections, not guarantees.

The progress page displays counts and prices only. It never exposes other students' names, phone numbers, flights or individual bookings.
