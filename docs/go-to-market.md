# Go-to-market: the pilot, and how it is priced

Written 2026-09-13. This exists so the reasoning is not rediscovered while
looking at a renewal.

## The motion

Walkthrough → sixty-day guided pilot on one project → annual contract.

No self-serve trial. That is a decision, not a gap, and the site says so in
those words.

### Why no trial

Researched 2026-09-13 against the vendors' own sites. Nobody in the category
offers one:

| | Publishes price | Trial | Self-serve |
|---|---|---|---|
| Bridgit Bench | no — `/pricing/` 404s | no | no |
| Assignar | no — "tailored… book a demo to get a custom quote" | no | no |
| Riskcast | no | no | no |
| Procore (incl. Workforce Planning, née LaborChart) | no — priced on Annual Construction Volume | no | no |

The tools that DO publish and offer trials sit an order of magnitude lower:
Contractor Foreman $49–$332/mo with a 30-day trial and self-serve checkout;
Workyard from $6/user/mo with 14 days. The dividing line is not "construction",
it is price point and who signs. Below ~$5k/yr one person spends their own
budget; above ~$10k/yr it is a committee and a budget line.

Buildertrend reportedly removed published pricing in 2026 for volume-based
quotes (third-party sourced; their pricing page returns 403). Nothing in the
category moved the other way.

### Why a trial would fail here specifically

- The board is empty until the roster, regions and projects are loaded. A trial
  that expires before that teaches the prospect the product is empty.
- The invite flow (`generatePasswordResetLink`) has never executed once.
- `seatLimit` returns a hard 402 that contradicts the "unlimited logins" the
  pricing page publishes five times.
- Provisioning is manual: tenant creation, and `tenants/{id}/meta/termsGate`.
- `delete-tenant.js` cannot finish without a service-account key, so an
  abandoned trial tenant is permanent clutter.
- One person cannot supervise concurrent unattended tenants.

## Pilot economics

**A converting Starter pilot is worth $3,600 in year one, not $4,800.**

    pilot fee                           $1,200
    credited against year one          -$1,200
    Starter annual                      $4,800
    ------------------------------------------
    cash in year one (converted)        $4,800   ($1,200 + $3,600)
    recognised as year-one contract     $3,600
    cash if it does NOT convert         $1,200

$4,800 is the renewal number, not the year-one number. Year two is the first
full-rate year. Anything compared against $4,800 in year one — payback period,
cost of acquisition, what a discount actually costs — is wrong by 25%.

Consequences worth holding:

- A discount offered on top of a pilot compounds against $3,600, not $4,800.
  Ten percent off is 13.3% of year-one revenue.
- A pilot that does not convert returns $1,200 against the loading work, the
  live walkthrough and two hours. Treat $1,200 as covering cost, not as margin.
- The same arithmetic at Growth ($9,600) gives $8,400 in year one; at
  Professional ($16,800), $15,600. The credit is flat, so it hurts Starter most
  in percentage terms — which is correct, because Starter is where a pilot is
  most likely to be the deciding factor.

## Published pricing — the argument both ways

Left as-is for now. Revisit on the triggers below.

**For keeping it.** At $4,800–$16,800 the buyer is one decision-maker spending
departmental budget. Published pricing does the qualifying a salesperson would
otherwise do, and walkthroughs are the scarce resource when one person runs
them all. It differentiates hard against a category that publishes nothing, and
buyers in this market notice.

**Against.** Tiers are headcount-based; Procore prices on Annual Construction
Volume. A contractor with 140 employees and $400M of work sees $9,600 and never
reveals they would have paid three times that. That anchoring loss is real.

**Which risk is larger today: anchoring — but not yet.** Anchoring loss is
measured against deals otherwise closed higher, and you cannot lose margin on a
deal you never get. Discovery cost is measured against walkthroughs you cannot
staff. With one person and no pipeline, the second binds.

**Revisit when:** a deal is lost on price discovery rather than fit; or a
prospect's headcount and construction volume disagree badly enough that
headcount is the wrong meter — the tell is a contractor whose employee count
says Growth while their volume says Professional.

**Enterprise is the exception.** Published and unsellable. It should be quoted,
and the honest reason is that the cost to serve one is unknown.

## What the product does about pilots: nothing new

A pilot is a plan plus a date, which is what the admin panel already models:

    /* A TRIAL IS A PLAN PLUS A DATE. Everything else behaves normally — limits
       still derive from the tier, add-ons still count. */

`trialStateOf()` badges the tenant list (`trial · 43d`, `trial expired`), shows
days remaining in the detail view, and FLAGS rather than locks — an expired
pilot is a conversation, not a reason to take a jobsite's board away.
Converting is clearing the date.

There is no `trial` tier in `PLAN_DEFAULTS` and there should not be;
`update-tenant.js` states why: "trialEndsAt is the only thing that makes it a
trial. A `trial` pseudo-tier would have no limits."

**Which field to use depends on whether money changed hands, and this matters:**

- **Paid pilot** (the model above): NOT a trial. It is an ordinary tenant with a
  short first term — use `contractStart` + `renewalDate`. Setting `trialEndsAt`
  would display "$0/yr — trial, not billing" on a customer who was invoiced
  $1,200.
- **Free pilot**, if one is ever granted: `trialEndsAt` is exactly right and the
  $0 display is then correct.

## What the site commits to

Every promise in "How it starts" is bounded and deliverable by one person: data
entry once per pilot, one live walkthrough, one hour at each end, "reachable"
rather than a response-time SLA. Deliberately excluded: integrations, custom
reports, configuration work, training materials, uptime commitments.

Two promises depend on things outside the copy:

- "complete export… deleted within thirty days" matches the privacy policy, but
  `delete-tenant.js` cannot currently finish without a service-account key.
- "no auto-renewal" is a commercial decision, not a code one.

**The copy does not state the pilot fee.** A prospect reads "pilot" and may
assume free. Decide whether $1,200 belongs on the page or in the conversation.
