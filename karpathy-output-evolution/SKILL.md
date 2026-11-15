---
name: karpathy-output-evolution
description: 'Choose and produce the output format that suits the reader: self-contained HTML or slides for a person viewing results, Markdown or JSON for agents, terminals and version control. Use this skill when the user wants better-formatted output, needs notes turned into a presentation, wants HTML output from an LLM, or says "make this prettier", "render this", "output as HTML", "make slides from this", "structured output", "Marp", "better format".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-education-first
  - karpathy-llm-wiki
  - karpathy-autoresearch
  - karpathy-idea-files
---

# Output Evolution

Choose the output format by who will read it. A person looking at results takes in visual, structured output faster than a wall of text, so for human readers HTML or slides often beat Markdown. For agents, terminals and files under version control, Markdown or plain data remains the right choice.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- The user asks for HTML, slides, a rendered report or a nicer format.
- A result will be shared, presented or compared visually.
- A long result holds tables or comparisons that are hard to read as text.

## When not to use

- A short answer the user will read once in the chat.
- Output that another program or agent will consume.
- Files that live as text in a repository, such as a README or docs, unless the user asks.
- The user has no way to open a browser where they are working.

## Inputs

- **The content** (required). Its substance must not change.
- **The reader and where they will view it.** If the user did not say and it is not obvious, pick a format from the table and say which in one line.
- **A format**, if the user named one.

## Procedure

### 1. Choose the format

| Reader and use | Format |
|---|---|
| Quick answer in a chat | Plain text or Markdown |
| An agent, a script or a pipeline | Markdown or JSON |
| A document kept in a repository | Markdown |
| A person reading a report, summary or comparison | Self-contained HTML |
| A person who needs to sort or filter data | HTML with a sortable, searchable table |
| A talk or a lesson | Marp slides |

### 2. Build it

**HTML**

- One file. CSS and JavaScript inline. No external resources (CDNs, web fonts, remote images), so it opens offline.
- Proper heading order; a table of contents when there are more than three sections.
- System fonts, a readable line width, usable on a phone.
- Interactivity uses standard browser APIs only.
- Escape any text that came from outside (user data, fetched pages) before placing it in the page. Never insert it as raw HTML.

**Research report (HTML).** Sections in this order: summary; findings (with a confidence level only where the input states one); evidence per finding; open questions; sources with links.

**Comparison (HTML).** The input's recommendation at the top, if it has one, then one card or column per option with the same criteria in the same order.

**Marp slides**

```markdown
---
marp: true
theme: default
paginate: true
---

# [Title]

[Author] · [Date]

---

## [One idea per slide]

- [At most five bullets]

<!-- Speaker notes go in an HTML comment like this one. -->
```

End with a slide of three to five takeaways.

### 3. Keep the content faithful

The output carries the same facts, numbers and caveats as the input. Do not add claims to fill a layout, and do not drop a caveat because it spoils one.

### 4. Deliver

Save to a file next to the source material, or where the user says, and tell the user its path and how to open it. A Marp file is Markdown: showing it as slides needs Marp (its CLI, or the VS Code or Obsidian plugin), so say that. If you have a way to render it, look at the result before delivering. If not, say it has not been checked visually.

## Output

One file in the chosen format, its path, and one line saying why that format.

## Done when

- [ ] The format matches the reader in the table, or the user's explicit choice.
- [ ] An HTML file opens with no network access.
- [ ] The content matches the input: nothing added, no caveat lost.
- [ ] The structure can be scanned: headings, tables, sections.
- [ ] The user was told whether the result was checked visually.

## Workflow

Step 3 of **Research to Publication**. It renders the wiki pages from `karpathy-llm-wiki` for their audience and passes the result to `karpathy-education-first`.

## Source

Karpathy's post of 2026-05-11: https://x.com/karpathy/status/2053872850101285137

- His tip is to ask the model to "structure your response as HTML" and open the file in a browser; he also reports success with slideshows.
- He expects output to progress from raw text, to Markdown (the current default), to HTML (an early but forming new default), and eventually to interactive neural video, which does not exist yet.
- His reason: vision is the fastest channel into the human brain.
- His closing line: "hot tip try ask for HTML".

He also has his knowledge-base agent render "slide shows (Marp format)" (2026-04-02): https://x.com/karpathy/status/2039805659525644595

The post is about output for people. The rule to keep Markdown or JSON for agents and repositories is this repository's addition.

The procedure, templates and thresholds here are this repository's elaboration.
