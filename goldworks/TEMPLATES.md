# Templates

The four shapes Goldworks writes. Copy, fill, delete the angle brackets.

## 1. An opportunity card — written by a miner, evidence only

```markdown
---
area: <area>
found: YYYY-MM-DD
sources_checked: <bare list of the surfaces consulted — including the ones that came up empty>
---
CLAIM: <one factual sentence, with the number that matters>.

EVIDENCE:
- <the fact in words + the proof: query and result, `file:line`, permalink, ID>
- <for a negative claim: the three surfaces searched, or the label "unverified — check in 10 seconds">

ACTIVITY: <active | recent | historical> — <recent window vs total, with the numbers>.

ROUGH ESTIMATE: value = <what it is worth and to whom> · cost for the owner = <their time only> ·
repeatable part = <what could be scripted>. PROBABILITY <low|medium|high> that <the third party>
cooperates, because <what happened last time> — only if the proposal depends on someone else.

PACKAGING: <what it would take for the owner to review this in two minutes>.
```

## 2. An issue — written by the judge when a card survives

```markdown
---
area: <area>
value: <1-10>
cost_owner: <the owner's time only — e.g. "5m (yes/no on a draft)">
deadline: <YYYY-MM-DD — hard dates only; omit otherwise>
follow_up: <YYYY-MM-DD — required on on-hold unless a trigger is set>
trigger: <textual trigger — required on on-hold unless follow_up is set>
owner: <who acts on it — the owner, or the person or team it belongs to>
source: <miner <area> YYYY-MM-DD · judge verification YYYY-MM-DD: what was re-checked>
bundle: <slug shared with related issues — omit if none>
---
<One sentence: what it is, with the number that matters.>

## The picture in two sentences
<the thing, its job, and the one fact that makes the rest self-evident>

## Evidence
<copied from the card, intact>

## The lever
<what would change, and who holds the lever>

## What the owner does
<the single gesture, sized to cost_owner: a word, a yes/no, a choice between two named options>

told: —
```

## 3. A dismissal — the card's body stays intact, the judge extends its frontmatter

```markdown
---
area: <area>
found: YYYY-MM-DD
sources_checked: <unchanged from the card>
dismissed: YYYY-MM-DD
reason: <why it died, one sentence>
revisit_if: <what would have to change to try again — a trigger a miner can check>
---
<the miner's original body, untouched: CLAIM / EVIDENCE / ACTIVITY / ROUGH ESTIMATE / PACKAGING>
```

## 4. A run file

```markdown
# GW run — YYYY-MM-DD

## Run 1 — HH:MM
- **Miner slots:** N of 5 available today · N used. world: <reused extract of YYYY-MM-DD | launched>.
- **Miners launched:** <area> (why). **Excluded:** <area> (why — recovered next run).
- **Triage:** N cards → N dismissed (<slug>: reason) · N promoted (<slug> → issue, value N) · N merged.
- **Lifecycle:** timers fired · promotions proposed · dismissed revisited.
- **Need to know:** chosen <…> · discarded <… why>.
- **Metrics:** acceptance rate of what reached the owner · their validation time · **factual errors: 0**.
DONE
```
