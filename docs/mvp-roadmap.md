# MVP Roadmap

## Product goal

Allow a student or family to submit flight and luggage details and immediately receive a transparent estimate, while operations and the vendor can see the same underlying departure and vehicle facts.

## Release 0 — validate rules

- Confirm capacity matrix with the vendor.
- Confirm whether fixed times are meeting or departure times.
- Confirm morning/afternoon cutoff.
- Confirm extra fees and cancellation policy.
- Test ten historical or realistic bookings by hand.
- Compare estimates with the RMB 1,800 family-of-three benchmark on an equivalent basis.
- Approve customer wording for “estimate” versus “confirmed price”.

Exit condition: no unresolved rule can cause unsafe capacity or an unexpected vendor charge.

## Release 1 — student intake and estimate

Student inputs:

- date;
- airline;
- flight number;
- scheduled arrival;
- party size;
- checked luggage;
- carry-on suitcases;
- latest acceptable departure;
- maximum budget;
- contact details;
- special items or needs.

Student output:

- airport-ready-time range;
- suggested fixed departure;
- current estimated booking total;
- estimated per-person amount;
- estimate expiry;
- explanation of what can change the price;
- submit and confirmation status.

Required validation:

- no negative counts;
- party and luggage combination checked against feasible vehicles;
- departure must follow conservative airport-ready time;
- over-budget estimate returns a clear alternative state.

## Release 2 — operations board

Views:

- Demand Inbox
- Departure Pools
- Assignment Comparison
- Price and Margin
- Exceptions
- Confirm and Lock
- Vendor Dispatch

Each pool card shows:

- passengers requested and confirmed;
- large and small luggage;
- suggested vehicle plan;
- vendor compensation;
- customer revenue;
- uncovered amount;
- markup;
- estimate confidence;
- hard-constraint alerts;
- confirmation deadline.

Operations actions:

- move demand between feasible departures;
- compare vehicle plans;
- override with a reason;
- lock a plan;
- generate student and vendor messages;
- reopen by creating a new version.

## Release 3 — vendor dispatch

Vendor workflow:

- receive dispatch request;
- accept, reject or suggest another vehicle;
- confirm compensation;
- submit actual vehicle, plate, driver and phone;
- mark ready;
- record changes and timestamps.

The vendor never sees student prices or company margin.

## Core statuses

### Demand

`draft → submitted → pooled → estimated → accepted → confirmed`

Alternative endings: `waitlisted`, `cancelled`, `expired`.

### Departure pool

`collecting → commercially-covered → ready-to-lock → locked → dispatched → departed → completed`

Alternative states: `at-risk`, `cancelled`.

### Vendor dispatch

`draft → sent → viewed → accepted → vehicle-assigned → ready → completed`

Alternative states: `rejected`, `superseded`, `cancelled`.

## Hard constraints for MVP

- passenger capacity;
- large-luggage capacity;
- small-luggage capacity where known;
- airport-ready time;
- latest acceptable departure;
- confirmed revenue coverage;
- valid price version;
- one final pool per booking;
- vendor acceptance before ready;
- no silent substitution of vehicle type.

## Soft preferences for MVP

- shortest reasonable airport wait;
- airline-affinity slot;
- keep families together;
- minimize number of vehicles;
- minimize unused capacity without approaching unsafe limits;
- avoid moving accepted bookings;
- prefer vendors with better historical reliability.

## Not in MVP

- automatic route optimization;
- multiple pickup addresses;
- live flight API;
- live GPS;
- vendor bidding;
- surge pricing;
- driver app;
- automated refund engine;
- AI-only dispatch decisions.

## First implementation backlog

1. Agree on data dictionary and rule versioning.
2. Produce student intake wireframe.
3. Produce estimate-results wireframe.
4. Produce departure-pool operations wireframe.
5. Produce vendor dispatch wireframe.
6. Create historical booking test cases.
7. Define acceptance tests for every hard constraint.
8. Select implementation stack only after the workflows are approved.


## Confirmed portal and authentication scope

The MVP has two entry points:

1. Public student site with secure booking progress.
2. Authenticated shared dispatch workspace for operations and vehicle vendors.

MVP authentication work includes:

- shared-workspace account/password login;
- secure server-side sessions;
- logout and expiry;
- dispatcher, vendor, manager and admin permissions;
- protected API authorization;
- audit events for login, pool lock/override and vehicle assignment;
- secure student booking reference.

The public site must not expose the dispatch workspace through a role switcher.

## Student progress MVP

The first release must let a submitted student view:

- current pooling status;
- confirmed passenger count;
- remaining safe capacity;
- current estimated per-person and booking price;
- “再来 X 人，预计人均价为 $Y” threshold;
- all feasible price projections for the current vehicle plan;
- luggage-capacity warnings;
- estimate version, update time and expiry;
- final locked price and dispatch information when available.

Projection values use only confirmed passengers. They do not reveal other passenger identities and do not promise that unconfirmed demand will join.
