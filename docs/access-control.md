# Access Control and Page Boundaries

## Decision

Lovable is a visual and interaction reference only. The production project remains in `dorothy110/ord-madison-shuttle`.

The product is delivered through three separate entry points. It must not use a public role switcher.

| Entry point | Audience | Authentication | Data scope |
|---|---|---|---|
| Student site | Students and families | Public intake; secure scoped link for returning booking | Only the current student's submission, estimate and confirmation |
| Operations portal | Internal staff | Required account and password | All operational and financial data allowed by staff permission |
| Vendor portal | Approved vendors | Required account and password | Only dispatches belonging to that vendor |

Exact production domains will be chosen later. Example structure:

- `www.example.com` — public student site
- `ops.example.com` — protected operations portal
- `vendor.example.com` — protected vendor portal

Separate domains are preferred for clarity, but separate protected route groups are acceptable if they enforce the same server-side boundary.

## Authentication requirements

- Store password hashes only; never store plaintext passwords.
- Use secure, HTTP-only, same-site session cookies.
- Rotate the session after successful login.
- Apply login rate limits and temporary lockout/backoff.
- Provide logout and session expiry.
- Do not reveal whether an unknown email/account exists in password errors.
- Password reset requires a time-limited, single-use token.
- Every protected request must be authorized on the server.
- Operations and vendor authorization must not rely on hidden buttons or client-side role values.
- Record successful login, failed login, logout, password reset and privileged actions.
- Multi-factor authentication is recommended for operations accounts and can follow after MVP unless risk assessment requires it immediately.

## Roles and permissions

### Student/public

Can:

- submit demand;
- receive an estimate;
- view/update a booking through a secure scoped reference;
- accept, decline or cancel according to policy.

Cannot:

- enumerate other bookings;
- view passenger lists;
- view vendor compensation;
- view company margin;
- access operations or vendor APIs.

### Operations staff

Initial roles:

- `ops_viewer`: view demand, pools and dispatch state;
- `ops_dispatcher`: edit pools, create assignments and dispatch vendors;
- `ops_manager`: Confirm & Lock, override rules and pricing with a reason;
- `admin`: manage staff/vendor accounts and permissions.

Sensitive actions require both permission and an audit record.

### Vendor user

Can:

- view dispatches assigned to the vendor organization;
- accept, reject or suggest a replacement;
- provide vehicle, plate, driver and phone;
- update allowed dispatch states.

Cannot:

- see customer-facing price;
- see margin;
- see other vendors;
- see unrelated dispatches;
- export the full student demand database;
- change vehicle or pricing rules.

## Vendor tenancy

Every vendor user belongs to one `vendor_id`. Every vendor-facing query must include and enforce that vendor scope on the server.

A guessed dispatch identifier must never allow access to another vendor's dispatch.

## Public booking privacy

A public booking lookup must use a random, non-sequential secure token or an authenticated student session. A numeric booking ID by itself is not sufficient.

Return the minimum necessary passenger information. Vendor dispatches should generally receive contact and passenger details only when operationally necessary.

## Suggested application boundary

A single repository can contain three web entry points plus shared rules:

```
apps/
  student-web/
  operations-web/
  vendor-web/
  api/
packages/
  business-rules/
  ui/
  types/
docs/
```

This is a planning structure, not a final framework choice.

Shared business rules may be reused, but authorization decisions remain in the API. The public student bundle must not contain operations pages or vendor-only data.

## Acceptance tests

- Opening an operations or vendor URL while logged out redirects to its login page.
- A student cannot call operations or vendor endpoints.
- A vendor cannot open another vendor's dispatch by changing an ID.
- A vendor response never contains customer price or margin fields.
- An operations viewer cannot lock a pool or override price.
- Logout invalidates the session.
- Expired and reset sessions cannot be reused.
- Privileged operations changes appear in the audit history.
