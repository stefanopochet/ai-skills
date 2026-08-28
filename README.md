# ai-skills

Skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code), packaged so they can be dropped
into another project.

Each skill lives in its own folder with a `README.md` (what it is, why it works, how to run it) and the
`SKILL.md` files Claude Code loads.

## Skills

| Skill | One line | Models |
|---|---|---|
| [`goldworks/`](goldworks/) | An **adversarial brainstormer that finds the work you didn't know about**: cheap AI miners dig through your systems and produce a mass of candidate findings, one expensive AI judge kills them by default, you only see what survives. Most of it is trash; the gold would never have reached you otherwise. | Judge: Claude Fable 5 `xhigh` (mandatory) · Miners: Claude Opus 5 `xhigh` |

## Installing a skill

1. Copy the skill's `skills/*` folders into your project's `.claude/skills/` (or `~/.claude/skills/` for
   every project).
2. Follow the skill's own `README.md` — some need a folder in the repo to keep their state in.
3. Invoke it with `/<skill-name>`.

Requirements: a recent Claude Code (the skills use background sessions, `--model` and `--effort`) with
access to the models each skill declares. Where a model is marked mandatory, don't downgrade it: the
README explains what breaks.

## Conventions

- Skills are **read-only towards the outside world** unless stated otherwise: they prepare, they never
  send messages, open tickets, deploy or write to external APIs.
- Everything a skill writes stays inside the repo it runs in, in plain markdown, so `git diff` shows what
  it did.
- State is files and folders: a folder is a state, moving a file is a transition. No database.

## License

Source-available, not open source: see [LICENSE](LICENSE). You may use, modify and run these skills,
including inside your own company. Redistribution requires attribution and a copy of the licence, and
offering them to third parties as a product or a hosted service requires written permission.
