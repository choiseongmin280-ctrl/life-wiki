# Life Wiki — <username>'s career & productivity knowledge base

An LLM-maintained wiki (pattern: [[llm-wiki-concept]]). Claude writes and maintains every page;
the user curates sources, directs analysis, and asks questions. Purpose: a compounding picture of
the user's career and work/study systems that can produce resumes, career feedback, and
recommendations on demand.

User: the wiki's owner (any field or career stage). Wiki language: English — preserve
original-language terms in parentheses when nuance matters. Browsed in Obsidian: use
`[[wikilinks]]` and YAML frontmatter on every page.
Scope: career + productivity workflow (projects, education, skills, experience, tools, routines,
learning). Personal-life topics (health, relationships) are out of scope.

At session start: read index.md, then the recent log (`grep "^## \[" log.md`, last ~5 entries).

## Three layers
- **Raw sources** — the user's files elsewhere on this computer, organized their way.
  Read-only inputs, read in place. Never move, copy, rename, or edit them.
- **The wiki** — this folder. Claude owns every file here. `llm-wiki-concept.md` is the founding
  doc: keep it unmodified.
- **The schema** — this file plus `templates/`. When conventions evolve, propose an edit and
  apply it only with the user's consent.

## Hard rules
1. **Privacy boundary.** Never list, search, glob, or read anything outside this folder.
   Exactly two exceptions: (a) the exact file or folder paths the user explicitly provides for
   ingestion (a folder grant covers listing/reading inside that folder, for that ingestion only);
   (b) existence-checks of paths already registered in sources.md (`Test-Path` only — never read
   content) during lint.
2. If a registered path no longer exists, ask the user for the new location. Never search the
   filesystem for it.
3. Cite sources by ID — `[[S001]]` — never by filesystem path. Absolute paths live only in
   sources.md.
4. **Template conformance.** Every page follows its category's file in `templates/` exactly —
   identical H1/H2 heading set, identical order, nothing added or removed; sub-headings (H3 and
   deeper) may be added within a section when its content needs structure. Read the template before
   creating or editing a page. Changing a format = schema op: update the template with user
   consent and migrate every existing page of that category in the same commit.
5. Every statement about the user's life or career must trace to a registered source or a dated
   user statement (`chat YYYY-MM-DD` in frontmatter `sources:`). No trace → don't write it.
6. **Ask, don't assume.** When a source leaves blanks — dates, outcomes, names, context — ask
   the user targeted questions during ingestion. What stays unanswered goes to
   open-questions.md, never gets silently filled in.
7. When the wiki lacks the data to answer, say "not in the wiki yet", list what to ingest, and
   stop — never fill gaps with plausible guesses. This rule binds resumes hardest.
8. When a new source contradicts an existing claim, keep both with dates and flag the conflict
   on the page (`> ⚠ conflict:`) — let the user resolve it.
9. End every operation by: updating index.md (its counts and stat lines must match the post-op
   state) → appending to log.md (always at the bottom — chronological order) →
   `git add -A && git commit`.
10. Web search is allowed for industry/market context (feedback, recommendations) — never as a
    substitute for wiki facts about the user.

## Layout
- `profile/` — overview.md (living synthesis), timeline.md (chronology), goals.md
- `career/education/` `career/experience/` `career/projects/` `career/skills/` `career/people/`
- `workflow/` — productivity systems, tools, routines, learning methods
- `sources/` — one summary page per ingested source, filename is the bare ID: `S001.md`
  (the descriptive title lives in the H1 heading inside, e.g. `# S001 — short title`)
- `insights/` — filed analyses: `YYYY-MM-DD-topic.md`
- `outputs/resume/` — `master.md` + tailored `YYYY-MM-DD-target.md`
- `templates/` — the fixed format for each category; the only place formats are defined
- `index.md`, `log.md`, `sources.md`, `open-questions.md`, `open-questions-archive.md` — described below
Create directories on first use. File names: kebab-case.

## Page conventions
Every page starts with:
---
type: entity | concept | source | insight | output | hub
entity: project | course | skill | experience | person | tool   # entity pages only
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [S001, chat 2026-06-10]
tags: []
---
Category → template mapping: project→templates/project.md, course→course.md,
experience→experience.md, skill→skill.md, person→person.md, tool→tool.md,
concept→concept.md, source page→source-summary.md, insight→insight.md, resume→resume.md.
Workflow pages use templates/tool.md (tools) or templates/concept.md (systems, routines,
methods). Profile pages, open-questions.md, and open-questions-archive.md are singletons — their
fixed structure is their existing file; preserve its headings exactly.
One page = one entity or concept. Open with the Summary section (1–3 sentences), details after.
Link liberally with `[[wikilinks]]`; a red link marks a page worth creating later, not an error.
Link & file hygiene (Obsidian resolves a link by path, filename stem, or frontmatter alias —
shortest match wins):
- One target per `[[...]]`: write `[[S016]], [[S039]]`, never `[[S016, S039]]`. When prose only
  mentions link syntax (log entries, examples), backtick-quote it so Obsidian doesn't parse it.
- A chat citation in prose is plain text — `(chat YYYY-MM-DD)` — never a `[[chat ...]]` wikilink;
  a dated chat is not a page, so the link would stay permanently red with a ghost node.
- A source page's filename is its bare ID (`sources/S0XX.md`) so `[[S0XX]]` citations resolve
  by filename stem — never `sources/S0XX-title.md`, which leaves every `[[S0XX]]` a red link
  with a ghost node in the graph. `aliases:` holds only human-readable names (titles,
  original-language terms), never the S-ID — the filename already is the S-ID. Never create a
  file elsewhere that shadows the citation (e.g. a root-level `S016.md` shadowing `sources/S016.md`).
- A 0-byte .md file is always a defect — Obsidian creates one when a red link is clicked.
  Populate it from registered sources or delete it; never leave it.
- A dead link whose entity already has a page under another name or folder is a wrong-path
  error: repoint it. Only links to genuinely not-yet-created pages stay red.
Every entity page ends with `## Open Questions` — unresolved gaps as `- [ ]` items, mirrored in
open-questions.md (and ticked everywhere once answered). profile/overview.md is the hub — keep it
current on every ingest; it is the first page read when generating any career output.

## sources.md — the registry
Append-only IDs (S001, S002, …). Entry format:

## S001 — Short title
- type: note | document | export | folder …  · ingested: YYYY-MM-DD · status: active | missing
- path: C:\Users\<username>\...\current\location.md
- verified: YYYY-MM-DD
- history:
  - YYYY-MM-DD registered at C:\Users\<username>\...\original\location.md

On any move: update `path` and `verified`, set status, append a history line. Never delete
history lines. IDs are never reused or renumbered. Sole exception: an Uningest removes the
entire entry, as if the ID had never been assigned.

## log.md
Append-only, oldest first — a new entry always goes at the bottom, never inserted at the top.
Entry header: `## [YYYY-MM-DD] op | short description` where op ∈
init | ingest | uningest | relocate | query | resume | feedback | recommend | lint | schema.
1–3 bullets of what changed beneath each header.

## open-questions.md & open-questions-archive.md
Split across two files, both `| Asked | Question | Page | Status |`, newest first:
- **open-questions.md** — only rows still `open` (kept short so it is cheap to read each session).
- **open-questions-archive.md** — resolved rows (`answered` | `dropped`); the full audit trail,
  not read during normal operation.
When the user answers one (any session): update the affected pages citing `chat YYYY-MM-DD`, tick
the page's checkbox, then MOVE the row out of open-questions.md to the top of
open-questions-archive.md with status `answered` (or `dropped`). Archiving, not deletion, is what
preserves the trail — never delete a resolved row.

## Operations

### Ingest — user gives path(s)
1. Read only the given path(s). Unreadable → report exactly what failed and stop.
2. Assign the next S-ID; add the registry entry (path, type, dates, first history line).
3. Present 3–5 key takeaways; fold in any emphasis the user gives.
4. **Gap interview.** List what the source leaves unknown (dates, outcomes, context, people,
   motivation). Ask the user up to 5 targeted questions now. Answers → `chat YYYY-MM-DD`
   sources; unanswered → open-questions.md + the page's `## Open Questions`.
5. Write `sources/S0XX.md` (filename is the bare ID; descriptive title goes in the H1) from
   templates/source-summary.md.
6. Create/update every affected page — profile pages, entities, concepts — from their
   templates, citing `[[S0XX]]`. Apply rule 8 to contradictions.
7. index.md → log.md → commit (`ingest: S0XX title`).

### Uningest — user retracts a source
1. Delete `sources/S0XX.md`, remove the registry entry whole, and delete or revert every page,
   claim, and open-questions row that traces only to S0XX. Content also backed by other sources
   stays, with the S0XX citation removed.
2. The wiki ends as if the source had never been ingested; only the log records that it was
   (log history is never rewritten — the uningest entry is the trace).
3. index.md → log.md → commit (`uningest: S0XX title`).

### Relocate — user reports a moved file
1. Update the registry entry: new path, verified date, status active, history line appended.
2. log.md → commit. (Pages cite IDs, so no page edits are ever needed.)

### Query — user asks a question
1. Read index.md; open only the relevant pages.
2. Answer with `[[page]]` and `[[S0XX]]` citations; honor rule 7.
3. If the answer is durable (comparison, analysis, decision), offer to file it under
   `insights/` via templates/insight.md — then index → log → commit.

### Resume
1. Read profile/* and all career/** pages (plus workflow pages if relevant to the target).
2. Missing essentials (education, contact, projects)? Output the gap list as the answer —
   what to ingest or answer to fill each gap — and stop. Rule 7 binds: nothing invented.
3. Write `outputs/resume/master.md` (or `YYYY-MM-DD-<target>.md` when tailored) from
   templates/resume.md; every line traceable to a page. Offer .docx/.pdf via available skills.
4. index → log → commit.

### Feedback & Recommendations
1. Read profile/goals.md, profile/overview.md, career/skills/*, career/projects/*, recent log.
2. Compare current state against goals: strengths, gaps, risks; next concrete actions.
   Optionally add market context via web search (rule 10).
3. Write `insights/YYYY-MM-DD-<topic>.md` from templates/insight.md; update goals.md only if
   the user confirms a change.
4. index → log → commit.

### Lint — on request; suggest if >1 month since last
1. Registry pass: Test-Path every registered path (existence only). Missing → status: missing,
   list for the user to re-point (rule 2).
2. Conformance pass: every page's H1/H2 headings match its category template exactly (H3+
   sub-headings within sections are allowed; violations listed with the diff); frontmatter
   complete and in inline-array form (`sources: [S001, chat 2026-06-10]` — never YAML block
   lists); every S-ID cited in a page body appears in that page's frontmatter `sources:`;
   every source page's filename
   is its bare ID (`sources/S0XX.md`, not `S0XX-title.md`) and its `aliases:` carries only
   human-readable names, not the S-ID.
3. Wiki pass: contradictions, claims superseded by newer sources (check source-page Summaries
   too, not only entity pages), orphan pages, entities mentioned twice+ without a page, stale
   goals.md.
4. Link pass — resolve every `[[link]]` exactly as Obsidian does (path, filename stem, or alias;
   text inside backtick code spans doesn't count):
   - malformed targets: `,`, `[`, or `]` inside `[[...]]`
   - source pages whose filename is `S0XX-title.md` rather than the bare `S0XX.md` — every
     `[[S0XX]]` citation goes red and spawns a ghost graph node; rename to `sources/S0XX.md`
   - 0-byte .md files anywhere (red-link click artifacts) — populate or delete
   - stray root-level .md files (only index, log, sources, open-questions,
     open-questions-archive, CLAUDE.md, llm-wiki-concept.md belong at root; anything else
     shadows link resolution)
   - classify every dead link: wrong path → repoint; target renamed → repoint; data already in
     a registered source → create the page; no data anywhere → replace with an open question.
5. Questions pass: no `open` rows in open-questions-archive.md and no resolved rows lingering in
   open-questions.md; resurface open-questions.md rows open 30+ days; checkbox mirror in both
   directions — every open row has a matching `- [ ]` on the page named in its Page column, and
   every archived (answered/dropped) row's checkboxes are ticked on every page that carries them.
6. Repo pass: `git status` must end clean. Modified or untracked leftovers mean an earlier
   operation skipped rule 9 — triage them, fold them into this lint's commit, note it in the log.
7. Report findings; apply the fixes the user approves.
8. log.md (op: lint) → commit.
