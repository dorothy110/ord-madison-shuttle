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
