# Life Wiki

A starter template for an **LLM-maintained personal knowledge base** — a structured, interlinked
collection of markdown files that an LLM agent (Claude Code, Codex, etc.) writes and maintains for
you. You curate sources and ask questions; the agent does the summarizing, cross-referencing,
filing, and bookkeeping. The wiki is a compounding artifact: it gets richer with every source you
add and every question you ask.

> **Credit.** The underlying idea — the "LLM Wiki" pattern — is **Andrej Karpathy's**. The founding
> document in this repo, [`llm-wiki-concept.md`](llm-wiki-concept.md), is his idea file, reproduced
> so the pattern travels with the template. See [Credits](#credits) for links. This repository is
> only one concrete instantiation of that pattern; the credit for the concept belongs to him.

## What this is

Most people's LLM + documents experience is RAG: upload files, retrieve chunks at query time,
generate an answer. The LLM rediscovers everything from scratch on every question — nothing
accumulates.

The LLM Wiki pattern is different. Instead of retrieving from raw documents each time, the LLM
**incrementally builds and maintains a persistent wiki** between you and your raw sources. When you
add a source, the agent reads it, extracts what matters, and integrates it — updating entity pages,
revising summaries, flagging contradictions, strengthening the synthesis. Knowledge is compiled
once and kept current, not re-derived per query.

Read [`llm-wiki-concept.md`](llm-wiki-concept.md) first — it explains the pattern in full. This
repo is a ready-to-use scaffold for one kind of wiki: a **career & productivity knowledge base**
(projects, education, skills, experience, tools, routines, learning). Repoint it at any domain by
editing the schema.

## What's in this repo

This is a **clean-slate template** — structure and conventions, no personal data.

| Path | Role |
|---|---|
| [`llm-wiki-concept.md`](llm-wiki-concept.md) | The founding idea doc (Karpathy's pattern). Keep it unmodified. |
| [`CLAUDE.md`](CLAUDE.md) | **The schema** — how the wiki is structured and the workflows the agent follows (ingest, query, lint, resume, …). This is the file you co-evolve with your agent. |
| `templates/` | The fixed page format for each category (project, course, skill, person, source, insight, …). Formats are defined here and nowhere else. |
| `profile/` | Living synthesis: `overview.md`, `timeline.md`, `goals.md` (empty stubs). |
| `index.md` | Content catalog of every page — the agent reads this first when answering. |
| `log.md` | Append-only chronological record of every operation. |
| `sources.md` | The source registry (append-only IDs: `S001`, `S002`, …). |
| `open-questions.md` / `open-questions-archive.md` | Unresolved gaps, and the resolved-question audit trail. |

Empty directories referenced by the schema (`career/`, `workflow/`, `sources/`, `insights/`,
`outputs/`) are created by the agent on first use.

## Getting started

1. **Get an agent.** Open this folder in [Claude Code](https://claude.com/claude-code) (or another
   coding agent). It will read [`CLAUDE.md`](CLAUDE.md) and adopt the schema.
2. **Adapt the schema (optional).** [`CLAUDE.md`](CLAUDE.md) is written for a career/productivity
   wiki. Edit its Scope, layout, and templates to fit your domain — research, a book, competitive
   analysis, whatever you're accumulating knowledge about.
3. **Ingest a source.** Point the agent at a file or note and ask it to ingest. It reads the source,
   discusses takeaways, writes a summary page, updates the index and affected pages, and logs it.
4. **Ask questions.** Query the wiki. Good answers (comparisons, analyses) can be filed back as new
   pages, so your explorations compound too.
5. **Browse in [Obsidian](https://obsidian.md).** The pages use `[[wikilinks]]` and YAML
   frontmatter; Obsidian's graph view shows the shape of your wiki as it grows.
6. **Lint periodically.** Ask the agent to health-check the wiki — contradictions, stale claims,
   orphan pages, broken links.

## Design notes

- **Three layers.** Raw sources (immutable inputs), the wiki (LLM-owned markdown), the schema
  (`CLAUDE.md` + `templates/`). See the concept doc.
- **Everything traces to a source.** Every claim cites a registered source ID (`[[S001]]`) or a
  dated statement. No trace → it doesn't get written.
- **Templates are law.** Every page conforms to its category template exactly; changing a format is
  a deliberate schema operation, not an ad-hoc edit.
- **Ask, don't assume.** When a source leaves blanks, the agent asks you or records the gap in
  `open-questions.md` — it never silently fills them in.

These conventions are what turn the agent into a disciplined wiki maintainer rather than a generic
chatbot. Adopt, drop, or rewrite them to taste — they live in [`CLAUDE.md`](CLAUDE.md).

## Credits

- **Concept & `llm-wiki-concept.md`:** [Andrej Karpathy](https://karpathy.ai/) — the LLM Wiki
  pattern.
  - Gist: <https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f>
  - Announcement: <https://x.com/karpathy/status/2039805659525644595>
- **This template** (schema, page templates, scaffold): built on top of that pattern for use with
  Claude Code.

## License

[MIT](LICENSE) — for this template (the schema, templates, and scaffold). The concept and
`llm-wiki-concept.md` originate with Andrej Karpathy and are included here with attribution.
