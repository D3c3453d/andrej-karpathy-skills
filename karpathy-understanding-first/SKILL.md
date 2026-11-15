---
name: karpathy-understanding-first
description: 'After AI-assisted work, make explicit what was assumed, what was actually verified, and what the human must inspect personally, so that understanding is not lost along with the typing. Use this skill when the user has used AI to build something and needs to verify it, wants to audit their own understanding, needs to know where LLM output is reliable, or says "verify this", "do I actually understand this", "what can I trust the AI on", "jagged capabilities", "understanding not outsourcing".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-llm-simulator
  - karpathy-system-prompt-learning
  - karpathy-meta-reflection
  - karpathy-practice-environments
---

# Understanding First

Handing work to an AI is fine. Losing track of what was built, and why, is not. After AI-assisted work, state what was assumed, what was actually verified, and what the human needs to look at personally.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- An agent produced work that the user is about to rely on or ship.
- The user asks whether the work can be trusted, or whether they really understand it.
- A decision rests on assumptions that have not been separated from facts.
- In a monthly review: finding recent AI-assisted work the user cannot explain.

## When not to use

- Trivial, low-stakes output.
- The user asked for the artefact only. Still flag, in one line, any unverified assumption that would be costly if wrong.

## Inputs

- **The work product**: a diff, a document, a decision, or a To verify list from `karpathy-llm-simulator` (required).
- **What it will be used for**, and what it costs if it is wrong.
- **The user's level** in the subject, if they will be audited on it.

## Procedure

### 1. Write the understanding report

Put this report in your reply. Do not write it into the user's files unless asked.

```markdown
## Understanding report
- Key assumptions: [each assumption the work depends on]
- Verified: [claim] - [evidence: test output, command result, document reference]
- Not verified: [claim] - [how to check it]
- Reliability notes: [the parts where model output is least trustworthy here, and why]
- Look at this yourself: [file and lines, setting, or number] - [what to look for]
```

Rules:

- For each claim, first try to check it with the tools you have: run it, read the source, fetch the primary document. What you could not check goes under Not verified.
- List under Verified only what was checked in this session, with the evidence. Rereading your own output and finding it plausible is not verification.
- Verify outside the model: run it, test it, compare it with the documentation, or go to the primary source. A model's statement of its own confidence does not count.
- Make `Look at this yourself` specific enough to act on: a file and line range, a configuration key, a figure.

### 2. Judge where model output is least reliable

These tiers are this repository's heuristics, not measurements:

| Reliability | Typical cases | What to do |
|---|---|---|
| Higher | Widely used languages and frameworks; standard algorithms; summarising or extracting from text that is provided | Verify by running or spot-checking |
| Lower | Library versions and APIs newer than the model's training; glue between several systems; long chains of reasoning; arithmetic done without a tool | Verify every part that matters |
| Lowest | Citations and references (they may be invented); niche or novel domains; legal, medical or financial specifics; anything costly if wrong | Confirm against an independent source |

### 3. Audit the user's understanding (when asked)

1. List the three to five key decisions in the work: technical choices, assumptions, trade-offs.
2. Ask the user to explain each in their own words, or to mark each as `can explain`, `shallow` or `black box`. Do not rate them yourself without asking.
3. For each `shallow` or `black box` item give a three-sentence explanation and one thing the user can check to confirm it.
4. For code the user wants to learn from: walk through the logic, say what would happen if a given part changed, and show a smaller version of the core idea.

### 4. Before shipping

Offer this checklist to the user:

```markdown
- [ ] I can say what this does in plain language without looking at it.
- [ ] I traced one example through it by hand.
- [ ] I know what a failure would look like.
- [ ] I ran it myself.
- [ ] If it breaks, I know where I would start debugging.
```

Any unchecked box means there is more to understand before shipping.

## Output

The understanding report. If an audit was requested, also the list of decisions with the user's own rating of each and the explanations given.

## Done when

- [ ] Key assumptions are listed explicitly, not left implicit in the work.
- [ ] Every item under Verified has evidence from this session.
- [ ] Every item under Not verified has a way to check it.
- [ ] The least reliable parts are named.
- [ ] `Look at this yourself` points to specific files, lines, settings or figures.
- [ ] If an audit was run, the ratings came from the user. When only the audit was requested, the report items above do not apply.

## Workflow

Step 2 of two workflows.

- **Debiased Decision:** it takes the To verify list from `karpathy-llm-simulator`, checks what it can, and reports. `karpathy-system-prompt-learning` follows only if the user has decided and wants a reusable rule.
- **Monthly Check-up:** it takes the skills marked KEEP SHARP by `karpathy-meta-reflection`. Ask the user for one or two recent AI-assisted pieces of work for each, and run only step 3 on them. The weakest area is the skill with the most `black box` ratings; pass it to `karpathy-practice-environments`. If the user has no work to show, say so and move on.

## Source

- "you can outsource your thinking but you cannot outsource your understanding" was written by @yacineMTB. Karpathy reposted it on 2026-04-30 as "the quote I've been citing a lot recently". https://x.com/karpathy/status/2049907410303865030
- On why model ability is jagged, Karpathy wrote the same day that it depends on how verifiable a domain is and on what the labs choose to train on, an explanation he calls unfinished: "You're either in the data distribution (on the rails of the RL circuits) and flying or you're off-roading in the jungle with a machete". https://x.com/karpathy/status/2049903821095354523
- On review as a separate skill, 2026-01-26: "Generation (writing code) and discrimination (reading code) are different capabilities in the brain." https://x.com/karpathy/status/2015883857489522876

The procedure, templates and thresholds here are this repository's elaboration.
