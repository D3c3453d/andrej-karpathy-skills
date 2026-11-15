---
name: karpathy-llm-wiki
description: 'Build and maintain an LLM-written knowledge base: a wiki of Markdown files compiled from sources the user collects. Use this skill when the user wants to organise research notes, build a second brain, set up an Obsidian vault maintained by an LLM, ingest papers or articles, ask questions against their notes, or mentions "personal knowledge base", "LLM wiki", "second brain", "knowledge management", "ingest papers", "obsidian + LLM".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-autoresearch
  - karpathy-output-evolution
  - karpathy-education-first
  - karpathy-idea-files
---

# LLM Wiki

You build and maintain a persistent wiki of Markdown files from sources the user collects. Knowledge is compiled once and kept current, instead of being worked out again for every question. The user chooses sources and asks questions; you do the summarising, cross-referencing and bookkeeping.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- The user wants a knowledge base set up for a topic they will keep adding to.
- A new source should be added to an existing wiki.
- The user asks a question that the wiki should answer.
- The wiki needs a health check.

## When not to use

- A one-off question with no wish to keep notes. Just answer it.
- A handful of files that fit in context. Read them directly.

## Inputs

- **Operation**: setup, ingest, query or lint (required; infer it from the request).
- **Wiki location** (required). If no wiki exists yet, propose the layout below and get agreement first.
- For ingest: the source file or files. For query: the question.

## Procedure

Read the wiki files yourself; do not ask the user to paste pages.

### Layout

```
[root]/
├── raw/         sources: read them, never edit them
├── wiki/        pages you write: summaries, entities, concepts, comparisons, an overview
│   ├── index.md   catalog of every page: link and one-line summary, by category
│   └── log.md     append-only record of what was done and when
└── AGENTS.md    the schema: structure, conventions, workflows (or CLAUDE.md)
```

This is a default. If the wiki already has a schema file, follow it. For the schema use the instruction file your host reads; if one already exists at the root, add a wiki section to it and never replace it.

### Setup

1. Agree the topic and the layout with the user.
2. Create the directories, `index.md`, `log.md` and the schema, and log the setup.
3. Record in the schema: page types, naming, link style, frontmatter, how sources are cited, the ingest steps. Update it with the user whenever a convention changes.

### Ingest

Take one source at a time unless the user asks for a batch.

1. If the source is not yet a file in `raw/` (a URL, pasted text, a report from another skill), save it there first as a new file. Adding files is allowed; changing an existing one is not. Then read it.
2. Tell the user the key points and ask what to emphasise. Skip this in batch mode.
3. Write a summary page for the source.
4. Update every existing page the source affects, including the overview; in a mature wiki one source might touch 10 to 15 pages. Create a new page only when no existing page fits.
5. Cite the source for every claim you add.
6. Where the source contradicts a page, keep both claims and mark them: `> CONFLICT: [old claim] ([source A]) vs [new claim] ([source B])`. If the newer source clearly supersedes the older, mark the old claim as superseded with the date. Never overwrite silently.
7. Update `index.md`. Append to `log.md`: `## [YYYY-MM-DD] ingest | [title]`.

### Query

1. Read `index.md` first, then the relevant pages.
2. Answer with citations to wiki pages. Say what the wiki does not cover. If you add anything from outside the wiki, label it as such.
3. If the answer is worth keeping, such as a comparison or an analysis, offer to file it as a new page. If the user agrees, add it, update the index and log the query.

### Lint

Run on request. Look for:

- contradictions between pages;
- stale claims that newer sources supersede;
- orphan pages with no inbound links;
- concepts mentioned often but lacking a page;
- missing cross-references;
- gaps a web search could fill;
- questions worth investigating and sources worth finding next.

Report as a table: issue, page, suggested fix. Apply fixes after the user approves, then log the pass.

### Practical notes

- Reading the index first works up to roughly 100 sources or a few hundred pages. Beyond that, suggest a local search tool over the Markdown files.
- Suggest keeping the wiki in git, so every change can be diffed and undone. Do not initialise a repository unasked.
- Keep durable knowledge in the wiki, not in an assistant's memory feature. (This is this repository's inference from Karpathy's remark on memory.)
- Obsidian is a convenient viewer, and its Web Clipper turns web articles into Markdown for `raw/`. Both are optional.

## Output

- **Setup:** the created structure and the schema.
- **Ingest:** a change list: pages created, pages updated, conflicts found, open questions.
- **Query:** the answer with citations and the gaps.
- **Lint:** the issue table.

## Done when

- [ ] Nothing in `raw/` was modified.
- [ ] Every claim added to a page cites its source.
- [ ] Contradictions are marked, not overwritten.
- [ ] `index.md` lists every page, and `log.md` has an entry for this setup, ingest, lint pass or filed answer.
- [ ] No page was created where an existing one could be updated.

## Workflow

Step 2 of **Research to Publication**. It saves the report and results from `karpathy-autoresearch` into `raw/` and ingests them, or is the first step when the research has no computable metric. The pages it produces go to `karpathy-output-evolution` to be rendered.

## Source

- Karpathy's post on LLM knowledge bases, 2026-04-02: "You rarely ever write or edit the wiki manually, it's the domain of the LLM." https://x.com/karpathy/status/2039805659525644595
- His LLM Wiki idea file gives the three layers (raw sources, wiki, schema), the three operations (ingest, query, lint), `index.md` and `log.md`. It calls the wiki "a persistent, compounding artifact" and notes that "good answers can be filed back into the wiki as new pages". https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- On assistant memory he wrote on 2026-03-25: "A single question from 2 months ago about some topic can keep coming up as some kind of a deep interest of mine with undue mentions in perpetuity." https://x.com/karpathy/status/2036836816654147718

The procedure, templates and thresholds here are this repository's elaboration.
