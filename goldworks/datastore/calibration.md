# Calibration — what passes and what doesn't

**What this is.** The rules of judgment, plus the archive of the verdicts they came from. **The judge and
every miner read this file at setup** — it is what makes the system yours rather than generic.

**How it grows.** Sections 1 and 2 ship with a starting set and grow with the owner's reactions. Every
time the owner reacts to something — including "I already knew that", including silence — add the rule to
the right section and the verbatim record to section 6. If a rule contradicts the archive, the rule wins.

---

## 1. Classes that never pass

Add a line each time the owner rejects a whole class, not just an item. These start the list; they hold in
most setups:

- **Reminders of duties already known.** The owner knows: the added value is zero.
- **Information they already have.** A vendor already in use, a document already on file.
- **Tasks that depend on someone else with unknown probability.** Without a motivated estimate that the
  person will respond, it is not presented.
- **Analyses without a lever.** "Interesting" is not enough: name the exact lever — which asset, which
  field, which setting — and ideally the causal why. Without it, it is not actionable.
- **Maintenance of things that are about to be retired.**
- **Theoretical cases never observed.**

## 2. What passes — the golden shapes

- **Something true they did not know, at zero effort for them.** The reference model.
- **An active defect inside their own responsibility**, with the numbers and the lever. Better still if it
  is money being lost right now.
- **Hygiene of the pipeline itself**: a scheduled script that broke, two documents that contradict each
  other.
- **Friction that can be removed for good** — a recurring block that could become a permanent rule.
- **A dated external signal**: a platform deadline, a policy change with a date attached.

## 3. The three rules that weigh severity

1. **Severity = impact × ownership × ACTIVITY.** Always the recent window next to the total (e.g. "5,000
   in total but 40 in the last 30 days"), and judge on the active part. A defect mostly in the past is a
   minor item, never an alarm.
2. **What is already covered is not flagged.** If the owner is on it, a NEW fact is required.
3. **Ownership.** Outside the owner's own responsibility, a finding is "good to know", never an alarm —
   however large the number.

## 4. Evidence rules

- Every claim with a concrete reference: query and result, `file:line`, permalink, ID.
- **Negative claims** need three forms of search on different surfaces, or the label "unverified". Read a
  thread to its end before calling anything unanswered or stuck.
- **Establish who the real actor is** before proposing anything that involves someone else.
- **Probability in the first line** when the proposal depends on someone's cooperation, with the reason.
- Anything that will cost the owner real time is verified twice before it is presented.

## 5. How what passes is presented

- **Plain language, linear story, no unexplained jargon.** Speak in outcomes.
- **Title, about three lines of explanation, and the value and cost scores, always stated.** Never a bare
  line.
- **Make it checkable in two minutes**: the exact reference (query, permalink, `file:line`) and, where
  possible, the raw output that shows the problem.
- **When proposing an alternative, say how it differs** from the option already on the table.

## 6. The `told:` rule and the archive of verdicts

Told once and ignored → silence until a timer, a trigger, or materially new evidence. Ignored twice →
never again. When something is ignored, record the **category**, not just the item: if a class of
reminders does not interest the owner, the bar rises for all of its siblings.

Keep the record below, newest at the bottom, one section per round:

```
## YYYY-MM-DD — <chat / pull / reaction to the digest>
- **<slug>** — verdict: "<their words, verbatim>" (score N) → rule: <what changed above, or "none">
- **Category:** <if a whole class was rejected or promoted, name it and the rule derived>
- **Told:** <what was communicated this round, and the reaction or the silence>
```
