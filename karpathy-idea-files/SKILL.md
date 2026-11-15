---
name: karpathy-idea-files
description: 'Write an idea down as a short spec, an idea file, instead of code, so that an agent or another person can build a version for their own setup. Use this skill when the user wants to capture a new idea quickly, needs a project spec for an agent to implement, wants to share a concept without building it first, or says "write up this idea", "create a spec", "gist this", "idea file", "agent can implement this", "brief for this project".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-agentic-engineering
  - karpathy-minimalism
  - karpathy-output-evolution
  - karpathy-llm-wiki
---

# Idea Files

Write the idea, not the implementation. An idea file is one Markdown document that explains an idea clearly enough for any capable agent to build a version suited to its owner's setup.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- The user has a new idea and wants it captured before any code is written.
- The user needs a brief that an agent can build from.
- The user wants to share a concept publicly without shipping an implementation.

## When not to use

- The task is already specified with success criteria. Go straight to `karpathy-agentic-engineering`.
- The user is reporting a bug or asking for a small change.

## Inputs

- **The idea**, in the user's own words (required).
- **Purpose**: is the file for the user's own agent to build from, or for sharing with others? Ask if it is not clear; it decides how specific the file should be.
- **Constraints** the user already knows (optional).

## Procedure

1. Restate the idea in two sentences. If you had to guess which problem it solves, confirm with the user before going on.
2. Fill in the template below. Do not invent decisions. Anything the user has not decided goes under Open questions.
3. Set the level of detail by purpose:
   - **For sharing:** keep it abstract. Describe the pattern; leave the stack, directory layout and tooling to the reader's agent.
   - **For the user's own build:** add concrete constraints and criteria.
4. Check that every success criterion is something a person or a script could observe.
5. Save the result as one Markdown file, `ideas/[slug].md` unless the user names another place. Do not write implementation code.
6. If the user wants it built now, hand the file to `karpathy-agentic-engineering`.

### Template

```markdown
# [Idea name]

## Summary
[Two or three sentences: what it is, which problem it solves, for whom.]

## Core insight
[The one non-obvious observation that makes this work.
If you cannot write it yet, make it the first open question.]

## How it works
[The pattern in prose: the main parts and how they interact. No code.]

## Success criteria
- [ ] [An observable outcome]
- [ ] [An observable outcome]
- [ ] [An observable outcome]

## Smallest useful version
[One paragraph: the simplest thing that would show the idea works.]

## Constraints (optional)
- Must: [...]
- Must not: [...]
- Out of scope: [...]

## Open questions
1. [Something not yet decided that affects the build]

## Notes for the implementer (optional)
- Suggested approach: [...]
- Main risk: [...]
- Prior art: [only things you have checked exist]
```

### Quick capture

When the user wants to note an idea in seconds, fill in only Summary, one success criterion and Open questions, and add `Status: draft` under the title.

## Output

- The path of the saved file.
- The open questions that need the user's decision, listed in the reply.

## Done when

- [ ] The summary says what the idea is and why it matters, without vague aspiration.
- [ ] Each success criterion is observable.
- [ ] Nothing the user had not decided was written as if decided.
- [ ] A reader with no other context could start from this file alone.
- [ ] The file contains no implementation code.

## Workflow

Step 1 of **Idea to Launch**. The finished idea file goes to `karpathy-agentic-engineering`, which uses its summary as the task and its success criteria as the checks.

## Source

- Karpathy introduced the idea file on 2026-04-04: "you just share the idea, then the other person's agent customizes & builds it for your specific needs". https://x.com/karpathy/status/2040470801506541998
- His own example is the LLM Wiki gist, whose closing note begins: "This document is intentionally abstract. It describes the idea, not a specific implementation." https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- On 2026-04-30 he gave a related example: "install .md skills instead of install .sh scripts". https://x.com/karpathy/status/2049903821095354523

His gist is free-form prose. The template with success criteria, constraints and open questions is this repository's addition.

The procedure, templates and thresholds here are this repository's elaboration.
