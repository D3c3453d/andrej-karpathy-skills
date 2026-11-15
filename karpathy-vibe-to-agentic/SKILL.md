---
name: karpathy-vibe-to-agentic
description: 'Take a quickly built, vibe-coded prototype and make it reliable: audit what was actually built, define acceptance criteria, and fix it in small reviewed steps. Use this skill when the user has a rough prototype and wants to level it up, needs to go from "it kind of works" to production quality, or says "polish this", "make this production-ready", "level up from prototype", "vibe coded this now what".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-agentic-engineering
  - karpathy-minimalism
  - karpathy-supply-chain-hygiene
  - karpathy-understanding-first
---

# From Vibe Coding to Agentic Engineering

Vibe coding produces a working prototype fast, without anyone reading the code. Before others rely on it, change mode: find out what was actually built, write down what ready means, and fix it one reviewed step at a time.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- A prototype exists and works roughly, and the user now wants to depend on it or ship it.
- The user asks to polish, harden or productionise something built quickly.

## When not to use

- No code exists yet. Use `karpathy-idea-files` or `karpathy-agentic-engineering`.
- The prototype is a throwaway and will stay one. Leave it alone.

## Inputs

- **The prototype**: its path, or the code (required).
- **What ready means**: who will use it, where it will run, what must not go wrong. Ask if unknown; the acceptance criteria depend on it.

## Procedure

### 1. Audit what exists

Read the code. Run it only after reading it, and only if it writes nothing outside its own directory and uses no real credentials or external service; otherwise ask first. Report bluntly:

1. What it does, in three to five sentences.
2. The assumptions built into it: hardcoded values, expected input formats, the machine it was written on.
3. The three most likely ways it fails.
4. What is missing for the intended use.
5. Whether fixing it is worth more than rewriting it.

### 2. Check for common prototype defects

| Defect | Fix |
|---|---|
| Hardcoded credentials, paths, magic numbers | Move to configuration or environment variables |
| No error handling on file or network operations | Handle the failure with a message that says what went wrong |
| Debug prints left in | Remove, or replace with logging |
| Functions doing several unrelated things | Split, but only where a fix needs it |
| Dependencies not declared in the manifest | Declare and pin them |
| Steps that only work by hand | Script them |
| No tests | Add tests for the core behaviour (step 4) |

A credential found in the code needs more than deletion. If the code was ever committed, pushed or shared, tell the user the credential must be rotated. Do not print its value in your report.

### 3. Define acceptance criteria

Write five to seven criteria, most important first, and get the user's agreement:

```text
GIVEN [input or condition]
WHEN  [action]
THEN  [expected result]
```

### 4. Pin down current behaviour

Before changing anything, add a test or a scripted check for what the prototype does correctly today. Without it you cannot tell whether a later fix broke the core.

### 5. Fix in small steps

1. Take the most critical issue only.
2. Make one change. Do not refactor unrelated code.
3. Explain WHAT changed, WHY it matters, and the RISK of the change.
4. Run the checks from steps 3 and 4 and show the output.
5. Wait for the user's approval before the next fix, unless they told you to continue on your own.

Stop when the acceptance criteria pass and each defect from step 2 is either fixed or listed as open.

### Quick upgrade of one small script

When the user wants a single pass over a short script instead of the full procedure:

- handle errors with clear messages;
- make hardcoded values arguments or environment variables, keeping the current values as defaults;
- replace debug and progress prints with logging, but leave prints that are the script's output;
- add a `--dry-run` flag if the script changes or deletes anything;
- keep the behaviour otherwise identical, and show the diff.

For this mode only the last three Done when items apply.

## Output

The audit, the agreed acceptance criteria, and after each fix: what changed, why, the risk, and the check results. At the end, which criteria pass and what remains.

## Done when

- [ ] The prototype's built-in assumptions are listed.
- [ ] Acceptance criteria exist in GIVEN/WHEN/THEN form and the user agreed to them.
- [ ] Core behaviour was pinned by a test before the first fix, and still passes.
- [ ] Each fix addressed one issue.
- [ ] Hardcoded values became configuration; any exposed credential was flagged for rotation.
- [ ] File and network failures are handled.

## Workflow

Step 5, the last, of **Idea to Launch**. It receives the built and audited project from `karpathy-supply-chain-hygiene` and hardens it for release. In this workflow the idea file's success criteria are the acceptance criteria and the existing tests count as the pinned behaviour; add only what is missing. On its own, it is the entry point when a prototype already exists.

## Source

- Karpathy coined vibe coding on 2025-02-02 as a way to "fully give in to the vibes, embrace exponentials, and forget that the code even exists", adding: "It's not too bad for throwaway weekend projects". https://x.com/karpathy/status/1886192184808149383
- On 2025-04-25 he described the opposite mode for code he cares about, with the emphasis on "keeping a very tight leash". https://x.com/karpathy/status/1915581920022585597
- On 2026-01-26 he wrote that with code you care about you should watch the agents "like a hawk", with an IDE open beside them. https://x.com/karpathy/status/2015883857489522876
- The line "vibe coding raised the floor. Agentic engineering raises the ceiling." is Stephanie Zhan's summary of her talk with him, which he quoted on 2026-04-30. https://x.com/karpathy/status/2049903821095354523

The procedure, templates and thresholds here are this repository's elaboration.
