# Access Control and Page Boundaries

## Decision

Lovable is a visual and interaction reference only. The production project remains in `dorothy110/ord-madison-shuttle`.

The product has two entry points:

| Entry point | Audience | Authentication | Data scope |
|---|---|---|---|
| Student site | Students and families | Public intake; secure scoped link for returning progress | Only the student's booking, pool progress and price projections |
| Shared dispatch workspace | Operations staff and approved vehicle vendors | Required account and password | Demand pools, vehicle economics, assignments and dispatch collaboration |

The protected workspace combines the former operations and vendor pages. Operations and vendors work together to form vehicle loads and assign vehicles.

Example domain structure:

- `www.example.com` — public student site
- `dispatch.example.com` — protected shared workspace

## Public student site

Students can:

- submit flight, party and luggage information;
- see their estimated airport-ready time and suggested departure;
- view their own pooling status;
- see the current confirmed passenger count;
- see how many additional passengers would lower the estimated per-person price;
- see price projections for feasible passenger counts;
- accept, decline or cancel according to policy.

Students cannot:

- view other passengers' names or contact information;
- view the full operations demand list;
- assign vehicles;
- view driver details before they are released for the confirmed trip;
- enumerate other bookings.

A booking-progress page must use a random secure token or authenticated student session. A numeric booking ID alone is not sufficient.

## Protected shared dispatch workspace

Operations and vehicle vendors use the same authenticated workspace.

They can collaborate on:

- reviewing incoming demand;
- grouping students into fixed departure pools;
- checking passenger and luggage capacity;
- comparing 7-seat and 15-seat plans;
- confirming simple vehicle economics;
- marking a pool ready;
- assigning a vehicle and driver;
- updating dispatch status;
- communicating schedule changes.

The shared workspace may display:

| Vehicle | Customer total | Vendor compensation | Gross margin |
|---|---:|---:|---:|
| 7-seat | $300 | $300 | $0 |
| 15-seat Mercedes | $650 | $600 | $50 |

The calculation is deliberately transparent because operations and the vendor jointly allocate vehicles.

## Accounts and permissions

The workspace is shared, but accounts remain identifiable.

Initial roles:

- `dispatcher`: review demand, create pools and propose vehicle assignments;
- `vendor`: accept a plan and enter vehicle/driver details;
- `manager`: lock/reopen pools and override a rule with a reason;
- `admin`: manage accounts and permissions.

A single person can receive more than one permission when appropriate. The interface does not need separate operations/vendor products, but privileged actions still require authorization and audit history.

## Authentication requirements

- Store password hashes only; never plaintext passwords.
- Use secure, HTTP-only, same-site session cookies.
- Rotate the session after successful login.
- Apply login rate limits and temporary lockout/backoff.
- Provide logout and session expiry.
- Do not reveal whether an unknown account exists in password errors.
- Password reset uses a time-limited, single-use token.
- Every protected request is authorized on the server.
- Record successful login, failed login, logout, password reset and privileged actions.
- Multi-factor authentication is recommended for manager/admin accounts after MVP unless risk assessment requires it sooner.

## Shared-workspace privacy

A vendor participating in the workspace may see the operational pool information needed to allocate vehicles. Student contact details must still be limited to what is operationally necessary.

The student public client must never receive the dispatch workspace data set. Hiding navigation in browser code is not access control.

## Suggested repository boundary

```
apps/
  student-web/
  dispatch-web/
  api/
packages/
  business-rules/
  ui/
  types/
docs/
```

Shared pricing and capacity rules can be reused by both web applications. Authentication and authorization remain enforced by the API.

## Acceptance tests

- Opening the dispatch workspace while logged out redirects to login.
- A student cannot call dispatch endpoints.
- A secure booking link exposes only its own booking and aggregate pool progress.
- Pool progress never reveals another student's identity.
- A dispatcher without manager permission cannot lock or override a pool.
- Vendor/vehicle assignment changes appear in audit history.
- Logout invalidates the session.
- Expired and reset sessions cannot be reused.
- The workspace displays $650 − $600 = $50 for a 15-seat plan.
