# Solo Founders and Consumer AI Products

Contents: 1) When this file applies · 2) What changes when one person is the whole org · 3) What to
drop or shrink · 4) What matters more · 5) The four layers, consumer edition · 6) Small samples,
honestly · 7) Telemetry instead of interviews · 8) Pricing the error when the user can't see it ·
9) Owner time is the scarce resource · 10) When the builders are agents · 11) Targets the owner
hasn't set · 12) Red flags in this setting

## 1. When this file applies

Read this when the "organization" is one founder, or a founder plus coding agents, and the users are
strangers who pay (or don't) per use: a consumer or prosumer AI app, an indie SaaS, a side project
with real money behind it. The rest of the skill was written around enterprise projects: a sponsor,
a business owner, staff users, a procurement queue. Most of its judgment transfers unchanged. Its
ceremony does not, and applying the ceremony to a one-person product produces documents nobody
reads and delays the only work that matters.

The rule of thumb: **keep every principle, shrink every artifact.** The reasons behind a RACI,
a UAT sign-off or a stakeholder map still exist. They just fit in one sentence now.

## 2. What changes when one person is the whole org

| Enterprise assumption | Solo / consumer reality | Consequence |
|---|---|---|
| A business owner signs acceptance | The founder is sponsor, owner, reviewer and on-call | "Who is accountable" is always answered; "who checks the accountable person" is not. An independent review (a second agent instance with fresh context, a written rule the founder set earlier) replaces the second signature |
| Users are staff you can interview and train | Users are anonymous strangers who leave silently | Interviews are rare and biased toward the vocal; behaviour data is the main evidence |
| Adoption can be mandated | Adoption is earned in the first session | Time to first value and what happens in the first minute dominate |
| Errors are caught by trained reviewers | Errors are caught by the user, who often lacks the expertise to notice | Unflagged wrong output is the most expensive failure (§8) |
| Budget is a project line | Every paid model call, eval run and tool is the founder's own money | Cost caps per run, and cost per result against price per result |
| Procurement and security review set the timeline | Third-party processors, privacy policy and payment rules set the obligations | The critical path is legal and platform terms, not a review board |
| Stop decisions are made by a steering committee | Nobody will stop the project except the founder | Written stop conditions with a date matter more, not less |

## 3. What to drop or shrink

Drop these, or reduce them to the line shown. Each one exists to coordinate people who are not here.

- **RACI and stakeholder map:** one line. "Owner: the founder. Executes: the founder and the agents
  the founder directs. Decides alone: X. Decides only with the founder's word: Y."
- **Project charter:** a one-page dated decision record. Append-only beats a living document,
  because the founder will want to see later what they believed then.
- **Procurement and third-party-risk review:** replaced by a list of the processors that touch user
  data, what each receives, and what the privacy policy says about it.
- **UAT with business sign-off:** replaced by watching unscripted real traffic and, where possible,
  the founder doing a blind review of outputs.
- **Pilot with a named team:** replaced by a staged release (canary, feature flag, a share of new
  sign-ups) with the exit criteria written before it starts.
- **Cross-team contracts:** become contracts with dependencies the founder doesn't control: the model
  vendor, an upstream library, a payment provider. Pin their versions; re-measure before switching.

Don't drop: baselines, the non-goals list, written stop conditions, the eval set, rules enforced in
code, rollback. Those protect the product, not the org chart.

## 4. What matters more

- **The first session.** Strangers decide in minutes. Measure time to first useful result and the
  share of first results that are actually good, not just "returned".
- **Unit economics per result.** Know the model cost of one result at the median and the 95th
  percentile, against what one result earns. A free tier or a free first result is a cost line with
  its own cap: the cost of all free results divided by the paying customers they produced is the
  real price of acquiring one, and it belongs in the same table as the paid margin.
- **Honest scope in the product itself.** In an enterprise the boundary of reliability lives in a
  launch memo. In a consumer product it must be visible where the user acts: what the product is
  reliable at, what it is not yet, and what an unfinished result looks like.
- **Abuse and free-tier gaming.** Anonymous users multiply accounts. Limits must be checked on the
  resulting state per person or device, not per request.
- **Public copy that stays true.** Every claim on a pricing page, a landing page or a terms page is
  a promise. A later change (a new limit, a removed feature) can make an untouched page false.

## 5. The four layers, consumer edition

The skill's four layers of evidence still apply. In a consumer product they look like this, and
one weekly page that puts all four side by side prevents the most common self-deception: a model
metric that rises while nothing downstream moves.

| Layer | Typical measures | Typical trap |
|---|---|---|
| Model quality | Share of results that pass your checks, by the hardest slice; consistency on repeated runs | The check is your own and can be wrong; the hard slice is averaged away |
| System | Time to result p50 and p95; share of runs stopped by a deadline or budget; error rate | Only the median is tracked, so the slow tail that loses users is invisible |
| Adoption | Activation, a second action after the first result, return on a later day | Counting sign-ups or results instead of the second action |
| Business | Paying customers (excluding friends and network), revenue against cost to serve | Treating one sale as validation of the whole funnel |

## 6. Small samples, honestly

A solo product lives on small numbers: a handful of payments, a few hundred first sessions, single-digit returns. The discipline is
the same as in the evaluation reference, with more weight on two habits:

- **Every number carries its denominator and its window.** "3 of 5 since Tuesday" is evidence;
  "60%" is not.
- **Set a read date and a minimum sample before looking.** Say in advance: "read when n ≥ 30 or on
  the 2nd, whichever is later". Peeking at n = 5 and changing direction is curve-fitting with extra
  steps.

When a decision cannot wait for a large enough sample, make it reversible and say so, rather than
pretending the small number settled it.

## 7. Telemetry instead of interviews

Interviews still beat guessing, but a solo founder rarely gets enough of them, and the people who
reply are not the people who left. So:

- **Instrument the funnel by stage** before redesigning it: arrival, the first action, sign-up
  completion, the wait, the first result, the next action, payment. A stage with no measurement is a
  stage where any story fits.
- **Read the failures, not the averages.** Pull the sessions that ended badly and classify them by
  where and why. The classification is free; it ranks the levers before any money is spent.
- **Every new event is a privacy decision.** Logging a new behaviour often needs the privacy policy
  to say so. Decide what is worth measuring, then widen the policy deliberately, not silently.
- **Production failures feed the eval set,** after removing personal data and checking the policy
  allows it.

## 8. Pricing the error when the user can't see it

The enterprise version of principle 4 asks who catches the error. In a consumer product the answer
is usually "the user", and often the user can't. A layperson can't see that a generated contract
misses a clause, a plan breaks a building rule, or a diagnosis summary drops a symptom. That makes
an unflagged defect the costliest error the product can make, worse than a slow or refused result.

The responses, cheapest first:

1. **Check what you can in code** and show the result: a pass, or a list of what is still wrong.
2. **Label unfinished output as unfinished.** A result produced because a deadline or budget ran out
   must never look like a normal result.
3. **Cut coverage, not depth.** If one slice (a harder input type) fails far more often, either say
   plainly in the product that it isn't reliable yet, or give it its own budget and flow. Don't let
   its failures be averaged into a headline number.
4. **Give a next step on a flawed result** (a free fix, a retry, a human channel), so the defect
   costs the user minutes, not trust.

## 9. Owner time is the scarce resource

In a solo product the founder's attention is the bottleneck, so route decisions deliberately:

- **Put a decision in front of the owner only when it changes error cost, money, or what users and
  customers see** (copy, pricing, legal text, anything sent to a customer, paid evaluation runs,
  releases). Everything else is decided by whoever executes, and recorded.
- **Bring the evidence with the question.** "Here is the failure table; option A or B; I recommend
  A because…" costs the owner one minute. "What should we do about quality?" costs an evening.
- **Record standing delegations in writing,** with their boundaries, so a later session doesn't
  re-ask what was already decided or exceed what was granted.

## 10. When the builders are agents

Many solo products are now built mostly by coding agents directed by the founder. Treat that
workforce with the same principles the skill applies to production agents:

- **Rules that must always hold live in code** (tests, hooks, CI gates, a guard on pushing to the
  deploy branch), not in an instructions file the agent may skim.
- **Review by an independent instance,** with fresh context and the code rather than the author's
  conclusion. An agent reviewing its own change rarely overturns it.
- **Risk tiers by what the change touches** (payments, auth, data deletion, public copy), not by
  who wrote it.
- **Briefs are complete.** A subagent starts with nothing; give it the goal, what is ruled out, the
  files, and what to return.
- **The person directing the agents is accountable for what they ship.** An agent's review is not
  the owner's review.

## 11. Targets the owner hasn't set

Founders often, and reasonably, decline to set a target before a baseline exists. Don't override
that. Instead, prepare the decision so it is cheap when the baseline arrives: after the scheduled
reading, write one page with a proposed threshold for each metric (a number, with a one-clause
reason), the date of the next reading, and what each outcome triggers (continue, shrink, stop). The
owner sets or rejects them. The point is that the threshold exists before the next result is seen.

## 12. Red flags in this setting

| You hear | What it usually means | Respond with |
|---|---|---|
| "It's just a side project, so we'll skip the evals" | Quality will be judged by the founder's last impression | A small fixed eval set and a read date; it can be 30 cases |
| "Pass rate is up, so it's working" | One layer read alone | The four-layer page: did adoption or payment move? |
| "Let's redesign the onboarding" when results are bad | Fixing the visible layer for a quality problem | Classify the failed results first; the interface can make a wait honest but can't make a result good |
| "We'll add a bigger model" before reading failures | Machinery before diagnosis | The failure table, then the cheapest fix that addresses the largest share |
| "Users would tell us if it were wrong" | They can't see the defect, or they just leave | Show what the checks found; label unfinished output |
| "We sold one, so the funnel works" | One sample | The written reopen or stop line, with its date and count |
| "The project is too small for that legal step" | A known obligation being deferred | Record what is accepted, the cost if it bites, and the trigger to revisit; it's the founder's call, made in writing |
| "The agent already reviewed it" | Self-review counted as review | An independent instance, or the owner |
