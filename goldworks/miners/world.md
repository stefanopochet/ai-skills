# Miner WORLD (pre-miner) — what is happening outside

**Special role: you run first and you produce no findings — you produce CONTEXT.** The other miners read
your extract before digging. You run at most once a day.

**Sources, in this order:**
1. The industry newsletters the owner subscribes to, through the mail API (so the run needs no browser).
2. Hacker News, through its public search API: the front page plus queries on the themes of the stack.
3. Release notes and changelogs of the tools the stack depends on — list them in the area's context file.
4. Targeted reading of whatever the first three surfaced: the actual release note, blog post or pricing
   page.

Sources that need a login are out of scope for an unattended run: skip them, write "world partial" in the
run file, and do not block.

**Output, in `gw/context/`:**
- `world-YYYY-MM-DD-raw.md` — the raw list: title, one line, link. Terse, not a blog post.
- `world-YYYY-MM-DD-extract.md` — only what is relevant to the owner's areas, with a `*` next to the hot
  items and, for every entry, *why it matters here* in one line.

**Lenses for the extract:** platform news that means money (ad formats, APIs, policies) · models and agent
tooling · software that touches the stack (deprecations, prices, vulnerabilities) · patterns worth
copying.

**Traps:** an extract entry without the "why it matters here" is noise; never trust a headline — if an
entry earns a `*`, open the source and verify it.
