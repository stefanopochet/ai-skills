# Goldworks

**An adversarial brainstormer for your backlog.**

Cheap fleets of AI *miners* dig through the systems you point them at and produce a mass of candidate
findings. One expensive AI *judge* kills them by default. You see only what survives, as a prioritized,
evidence-backed backlog you read when you want.

**Most of what the miners produce is trash — a 99% failure rate is fine — and the gold would never have
reached you otherwise.**

Goldworks runs on [Claude Code](https://docs.anthropic.com/en/docs/claude-code) and ends at the backlog:
what you do with an issue is your call, and it never acts on the world — no messages, no tickets, no
deploys, no API writes.

---

## Why it works — and when it doesn't

Classic brainstorming says *suspend judgment*. Goldworks inverts it: mass generation and ruthless judgment
coexist, because the generators are machines without ego.

1. **Cheap fleets generate.** A high failure rate is fine: a failed miner costs almost nothing.
2. **One high-end judge kills by default.** The judge is the quality ceiling of the system — improving it
   pays more than enlarging the fleet.
3. **Whoever generates does not judge.** Separate contexts, always: a model asked to grade its own
   candidates grades them well.
4. **Ground truth at every step.** Every claim carries its evidence (a query and its result, a
   `file:line`, a permalink). A negative claim ("it never shipped") needs three searches on different
   surfaces or it is not asserted.
5. **You see only the survivors.** The dead are not lost: every dismissal keeps a *reason* and a *revisit
   trigger*, so the same corpse is never proposed twice.
6. **Cheap verification is the condition of applicability** — the
   [asymmetry of verification](https://www.jasonwei.net/blog/asymmetry-of-verification-and-verifiers-law):
   some tasks are far easier to check than to solve, and those are the ones this works on. Where
   verification is expensive or impossible, don't point Goldworks there.

One corollary that is easy to get wrong: **select, never aggregate.** The judge picks one candidate; it
never votes or averages, because at high failure rates a majority selects the *modal error*.

---

## Architecture

| Role | Skill | Model | Effort |
|---|---|---|---|
| **The judge** | `/gw` | **Claude Fable 5** (`claude-fable-5`) | **`xhigh` — mandatory** |
| **The miners** | `/gw-miner <area>` | **Claude Opus 5** (`claude-opus-5`) | **`xhigh`** |

**Why the model rule is not negotiable.** The judge is where the errors that matter happen: a false
promotion costs your attention, a false kill costs the gold. Everything else in the system assumes the
judge is that good. Miners are the opposite — many, wrong most of the time by design — and Opus 5 at
`xhigh` is the sweet spot between cost and evidence quality. (On Opus we recommend `xhigh` specifically:
in our own measurements the intermediate effort levels performed worse for this kind of work.)

### The lifecycle

```
miners                the judge (Fable xhigh)                    you
──────                ───────────────────────                    ───
opportunities/new/ ─▶ verify, then default KILL ─▶ dismissed/  (reason + revisit trigger)
                                               └▶ issues/backlog/
                                                        │  proposed by the judge,
                                                        ▼  approved by you
                                                   issues/todo/ ─▶ done/  or  cancelled/
                                        on-hold/ = waiting on someone — always with a date or a trigger
```

- A **card** is what a miner writes: one file, evidence only, no verdict.
- **Triage** is the judge's first job: verify (re-run the query, read the thread to its end), then kill by
  default. Survivors become an **issue** with `value` (1-10), `cost_owner` (your time only, net of what you
  can delegate to AI), an optional `deadline`.
- `backlog → todo` is proposed by the judge and **approved by you**: intent is yours.
- **The folder is the state.** Moving a file is the transition.

### A run, end to end

1. **State** — read `state.json` (pause key, concurrency guard, which miners already ran today) and
   `calibration.md`.
2. **Miners** — at most 5 a day, chosen by rotation on the least recently run areas. Each is a background
   session the judge dispatches and supervises; one that dies is excluded and noted, never escalated to
   you.
3. **Triage** — dedup against `dismissed/` and the open issues; verify; weigh severity on the *active*
   window; default KILL with a reason and a revisit trigger; survivors go to the backlog, rewritten so
   that your share of the work is as small as possible.
4. **Lifecycle** — expired timers, approaching deadlines, dismissed cards whose trigger may have fired.
5. **Output** — regenerate `INDEX.md`, pick the *need-to-know* (usually nothing), write the run file.

Zero findings in a run is a successful outcome. Digging less so there is less to reject is the worst
failure.

---

## How to run it

1. **Install the skills.** Copy `skills/gw/` and `skills/gw-miner/` into `.claude/skills/`.
2. **Create the state folder.** Copy `datastore/` to `<your-repo>/gw/`.
3. **Choose your areas.** Copy the configs you want from [`miners/`](miners/) into `gw/miners/`, then
   write your own the same way — sources, lenses, traps. Four are ready to use:

   | Miner | Digs | Typical finding |
   |---|---|---|
   | `meta-ads-competitors` | the public Meta Ad Library | an angle a competitor has been running unchanged for 90 days — i.e. one that pays |
   | `meta-ads-performance` | your own ad accounts | spend that grew while the outcome that matters stayed flat |
   | `machine-health` | the host the automation runs on | a scheduled job that has been failing for weeks in a log nobody reads |
   | `world` | the news that touches your stack | a pre-miner: it produces context for the others, not findings |

4. **Seed the calibration.** `gw/calibration.md` ships with the method rules. Add your own verdicts as
   they come — that file is what makes the judge *yours*.
5. **Let background sessions write into the checkout.** If your setup isolates background sessions from
   the working tree, they cannot deposit their cards. In recent Claude Code versions this does it:
   ```json
   { "worktree": { "bgIsolation": "none" } }
   ```
   in `.claude/settings.json`. It only controls where background sessions write; nothing is built.
6. **Launch the judge**, interactively with `/gw` or in the background:
   ```
   claude --bg --model claude-fable-5 --effort xhigh --name "gw" -- "/gw"
   ```
   It dispatches the miners itself:
   ```
   claude --bg --model claude-opus-5 --effort xhigh --name "gw-miner-<area>-<YYYY-MM-DD>" -- "/gw-miner <area>"
   ```
   `/gw triage-only` judges what is already on disk without launching miners. `/gw pull: <request>`
   answers "I have some time, give me three tasks".
7. **Read `INDEX.md`, give verdicts.** Your reactions — including "I knew that already" — go into
   `calibration.md`. That is how the bar rises.

---

## What a card looks like

A miner deposits evidence and nothing else. Invented example, from the competitor miner:

```
---
area: meta-ads-competitors
found: 2026-03-14
sources_checked: Ad Library, 6 advertisers in the category (one country filter), all active ads · the 3
  landing pages the winning ads point to · the dismissed cards of this area
---
CLAIM: Two of the six advertisers have been running the same "free returns" angle unchanged for 94 and 71
days, while the owner's angles are replaced within 3 weeks. Longevity that long is the strongest public
signal that an angle pays.

EVIDENCE:
- Advertiser A, ad id …, first seen 2025-12-11, still active today: 94 days, 11 near-identical variants,
  same headline, only the opening frame changes.
- Advertiser B, ad id …, first seen 2026-01-03, still active: 71 days, 4 variants.
- The other four advertisers rotate every 12-25 days: no angle survives past a month.
- Both winning ads land on a page whose first screen states the return policy; the owner's page shows it
  in the footer.

ACTIVITY: active — both ads were still serving at the time of the check.

ROUGH ESTIMATE: value = an angle validated by someone else's money, ready to be adapted, plus a landing
page change that can be tested in a day. Cost for the owner: 10 minutes to look at the two ads and say yes
or no. Repeatable part: the weekly longevity ranking of the whole category.

PACKAGING: the two ads side by side with their run length, and the one-line difference between their
landing page and the owner's.
```

The judge then verifies it, scores it, and either kills it with a revisit trigger or turns it into an
issue. Most cards die. That is the point.

---

## Rules that are not negotiable

- **The judge runs on Claude Fable 5 at `xhigh`.** Miners on Claude Opus 5 at `xhigh`.
- **Whoever generates does not judge.** Never the same context.
- **Default KILL.** A high bar for what reaches you; zero findings is a fine day.
- **Ground truth.** Evidence with references; negative claims only after three searches on different
  surfaces; when a proposal depends on someone else's cooperation, its probability goes in the first line.
- **Severity = impact × ownership × activity.** Always the recent window next to the total: a defect that
  is mostly in the past is a minor item, never an alarm.
- **Read-only towards the outside.** Goldworks prepares; it never sends, posts, deploys or writes.
- **Never aggregate verdicts.** Select.

## Layout

```
goldworks/
├── README.md                this file
├── TEMPLATES.md             card · issue · dismissal · run file
├── skills/gw/SKILL.md       the judge
├── skills/gw-miner/SKILL.md the generic miner
├── miners/                  four ready-to-use area configs
└── datastore/               the gw/ skeleton to copy into your repo
```
