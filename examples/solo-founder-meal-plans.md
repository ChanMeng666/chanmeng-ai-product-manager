# Solo Founder, Allergy and Medical-Diet Meal Plans: What to Do Next Month

**What I understood:** you're one founder directing coding agents and you have about 6 hours a week. That's roughly 24 hours next month. You have 400 sign-ups in 6 weeks and 9 paying customers. 4 of them are friends, so 5 are strangers: 1.25% of sign-ups. About 6% of users (around 24 people) ever make a second plan. Your users have allergies, coeliac disease, low-FODMAP or renal diets. On multi-allergy diets your own checker flags about 30% of plans, and those plans are still shown with a small icon.

## Verdict

**Don't redesign onboarding or switch to a bigger model this month.** First, make sure a plan that fails your allergen check can never look like a normal plan. Enforce that in code. Then find out *why* plans fail and *why* people don't come back. Those are cheap queries and a few hours of reading. Decide on onboarding and the model only after that, with the evidence in hand.

**Don't write a PRD or a RACI.** Write one dated page of decisions (below). A RACI for one person is one line.

## Why

**The flagged plans are the most expensive thing in your product.** For someone with a nut allergy or coeliac disease, a wrong ingredient isn't an inconvenience. It can be a reaction, sometimes a serious one, and it can't be undone. Your users mostly can't tell which item in a 7-day plan is the problem. That's why they rely on you. A small warning icon on a plan that otherwise looks complete is a reminder, not a gate. Some users will cook from it anyway. It's also your strongest churn suspect: a person who has to re-check every line of your plan has gained nothing and won't buy a second one.

**Onboarding can't fix a bad first result.** A 6% second-plan rate has three possible causes: the first plan wasn't trusted, wasn't good, or wasn't needed again. Onboarding only touches how people *reach* the first plan. You can test this today, for free: compare the second-plan rate of users whose first plan was flagged with users whose first plan passed. If the flagged group comes back much less, the problem is quality, and a redesign will only be polishing the entrance.

**A bigger model before diagnosis is guesswork that costs money and time.** You don't yet know whether the 30% comes from the model (it ignores the constraint), from the checker (false alarms), or from missing data (the model doesn't know that this stock cube contains wheat). A bigger model fixes only the first cause. It will almost certainly be slower than your 95 s median and cost more per plan, and you don't know your p95 or cost per plan against the $4 you charge. Allergen exclusion is mostly deterministic, too. "Never include an ingredient that contains X" can be enforced in code before or after generation. Rules beat a smarter guesser here.

**Your checker is also a model of the truth, and it can be wrong in both directions.** The 70% that pass aren't proven safe. Some unflagged plans may contain an allergen your checker misses: hidden wheat in soy sauce, "may contain" labels, or potassium and phosphorus limits for renal diets that the checker may not check at all. Unflagged wrong output is worse than flagged wrong output, because nobody is warned.

## Red flags, most expensive first

1. **A known-unsafe output is shown as a result.** For a health product this is the red line. The fix belongs in code: a plan that fails the check is repaired and re-checked, or it's withheld with a plain statement of what still conflicts. It's never shown as a finished plan.
2. **Health data and medical-adjacent claims.** Allergies and medical diets are health data. In the UK and EU that's special-category data under GDPR. Know which processors receive it (the model provider, analytics, email) and what your privacy policy says. Check your public copy too: "safe for coeliacs" or "renal diet plan" is a promise. Get advice for your jurisdiction on where meal planning ends and medical advice begins, especially for renal diets, where limits are set per patient by a clinician.
3. **Machinery before diagnosis.** You're planning a bigger model without a failure breakdown.
4. **Fixing the visible layer for a quality problem.** This is the onboarding redesign.
5. **Only the median is tracked.** The slow tail is where strangers give up during the wait, and you can't see it.
6. **Friends counted as validation.** The honest paying number is 5 strangers in 6 weeks. That's fine at this stage, but it's the number to track.
7. **No stop line.** You're 6 weeks in with no written condition for continue, shrink or stop. Write it now, while you're calm.

## The one page: decision record (replaces the PRD and the RACI)

> **Meal planner: decisions, written 2026-11-27. Append only; don't edit old entries.**
>
> **Owner:** me. **Executes:** me and the agents I direct. **Agents decide alone:** code structure,
> tests, internal refactors, anything not listed next. **Only with my word:** anything touching
> the allergen check or diet rules, payments and credits, privacy policy and public copy, a model
> or prompt change in production, any paid eval run.
>
> **Red line (not negotiable):** a plan that fails the ingredient check is never shown as a normal
> plan. Proposed threshold: 0 such plans per week, checked in code and counted in a weekly query.
>
> **Non-goals for December, each with its reason:**
> - Onboarding redesign: until the flagged-vs-passed comparison shows the first result isn't the cause.
> - Bigger model: until the failure table shows the model ignoring constraints is the largest cause,
>   and it beats the current model on the eval set at an acceptable cost and p95.
> - New diets or features: each diet needs its own checked rules; depth before coverage.
> - Renal diets presented as reliable: [ASSUMPTION: the checker doesn't check potassium,
>   phosphorus or sodium]. Until it does, the product says plainly that these plans are not yet
>   checked against renal limits and must be agreed with your renal team.
>
> **Baselines to record by 2026-12-07:** time to plan p50 and p95; model cost per plan p50 and
> p95; flag rate by diet type (denominator: plans in the last 6 weeks); second-plan rate, flagged
> first plan vs passed first plan; paying non-friend customers.
>
> **Proposed targets (each one my call to accept or change):**
> - Multi-allergy plans shown to users that still fail the check: 0. This is the red line.
> - Multi-allergy plans that need a repair or are withheld: ≤ 10% on the eval set. Proposed
>   reason: at 1 in 10 a user still feels the product works.
> - Second-plan rate among users whose first plan came after the fix: ≥ 12%, read when n ≥ 100
>   such users or on 2027-01-15, whichever is later. Proposed reason: double today's rate is a
>   movement small noise can't fake at that n.
>
> **Stop / shrink line:** if by 2027-02-28 there are fewer than 15 paying non-friend customers
> *and* the second-plan rate after the fix is still below 10%, I shrink to a single diet (the one
> with the best second-plan rate) or stop. I won't add features to rescue the numbers.

## Next month in about 24 hours

| Week | Hours | What | Done when |
|---|---|---|---|
| 1 | 6 | **Close the red line in code.** If the check fails, feed the failing items back for one repair pass and re-check. If it still fails, don't show the plan as finished: show "This plan isn't ready: these 2 items conflict with your dairy allergy" and refund the credit. Add a test that fails if a flagged plan reaches the ready state. Have an agent instance with fresh context, not the one that wrote the change, review it. **Also run three queries:** p95 time to plan, cost per plan, and second-plan rate for flagged vs passed first plans | The test exists and you've seen it go red when you remove the gate; three numbers recorded with their denominators |
| 2 | 6 | **Read the failures.** Take 40 flagged multi-allergy plans and sort each into one cause: checker false alarm / model ignored the constraint / hidden ingredient (a compound product like stock or sauce) / missing data or rule / other. Then blind-review 20 *passed* multi-allergy plans yourself, looking for allergens the checker missed | A failure table with counts out of 40, plus a count of misses out of 20 |
| 3 | 6 | **Build a fixed eval set of 30 to 40 real briefs** (personal details removed, and only if your privacy policy allows it), weighted toward multi-allergy and renal. **Fix the largest cause at the cheapest layer:** an allowed-ingredient list enforced in code, a better ingredient database for compound products, clearer constraints in the prompt. Try the bigger model *only* if "model ignored the constraint" is the largest row. Run it on the same eval set with versions pinned and a cost cap you set before running | Eval-set flag rate, before vs after, on the same 30 to 40 cases |
| 4 | 6 | **Weekly one-page readout,** all four layers side by side: check pass rate by diet; p50/p95 and cost per plan; second-plan rate; paying strangers. Fill in the privacy processor list. Then decide whether onboarding is next, with the flagged-vs-passed comparison in hand | The decision record has a new dated entry |

If week 1's comparison shows that users with *passed* first plans also rarely return, the problem may be that a weekly plan isn't needed again. In that case, onboarding and pricing become more relevant. That's a good result to find out cheaply.

## Assumptions that would change this

- **[ASSUMPTION]** The checker covers allergens but not renal nutrient limits. If it does cover them, drop the renal non-goal.
- **[ASSUMPTION]** A bigger model would raise cost per plan noticeably against the $4 price. If your current cost is pennies and latency is fine, the model test in week 3 gets cheaper, but it still comes after the failure table.
- **[ASSUMPTION]** You sell in the UK or EU. If you sell in the US, the health-data rules differ, but the red line doesn't.

## Single next action

**You, this week (2 hours):** have an agent write the gate that stops a flagged plan from reaching the ready state, with a test you watch go red and then green. Have a separate fresh agent review it. In the same session, run the flagged-vs-passed second-plan query. Those two things tell you more about December than any PRD would.
