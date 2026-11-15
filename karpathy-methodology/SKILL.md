---
name: karpathy-methodology
description: 'Router for the Karpathy-inspired skill pack: picks which of the 14 skills or 4 workflows fits the task and loads it. Use this skill when the user wants a Karpathy-style approach but has not named a specific skill, asks which skill to use, or names one of the workflows (Idea to Launch, Research to Publication, Debiased Decision, Monthly Check-up). Also triggers on: "karpathy method", "karpathy approach", "karpathy way", "AK style", "like karpathy does", "karpathy principles", "which karpathy skill".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-agentic-engineering
  - karpathy-llm-simulator
  - karpathy-llm-wiki
  - karpathy-autoresearch
  - karpathy-meta-reflection
---

# Karpathy Methodology (router)

This skill does no task work itself. It picks the right skill or workflow from this pack and hands over to it. Most tasks need one skill, sometimes two or three. Never load all of them.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- The user asks for a Karpathy-style approach without naming a skill.
- The user asks which skill fits their task.
- The user names one of the four workflows listed under Workflow, or asks for a whole chain end to end.

## When not to use

- The user named a specific `karpathy-*` skill. Load that skill directly.
- The request matches no row in the Skills table. Answer normally; do not force a skill onto it.

## Inputs

- The user's request (required).
- If two rows fit equally, ask one question: what result do they want to hold at the end?

## Procedure

1. Match the request to one row of the Skills table.
   - Choose a workflow only if the user names it or asks for the whole chain end to end.
   - If the request has two separable parts, run the two skills in the order asked, under the rules of step 5.
   - If the user only asked which skill fits, name it, say why, and stop.
   - If no skill fits, say so and answer normally.
2. Tell the user in one line which you chose and why.
3. Load it. To load a skill, invoke it by name if your host supports skills. Otherwise read `../[skill-name]/SKILL.md`, relative to this file (the skills are installed side by side). If neither works, say the skill is not installed and ask how to proceed. Do not work from the one-line summary in the table.
4. Follow the loaded skill's Procedure until its Done when list holds.
5. For a workflow, run its skills in the listed order:
   - load each skill when its turn comes (one exception is listed under Workflow);
   - pass the previous skill's Output to the next skill as its Inputs;
   - move on only when the current skill's Done when list holds, or the user says to skip it;
   - if a step has nothing to act on, say so in one line and move on;
   - tell the user at each handover which step is starting.

### Skills

| Skill | Load when the user wants to |
|---|---|
| `karpathy-idea-files` | write an idea down as a spec before any code, or share an idea without an implementation |
| `karpathy-agentic-engineering` | build or change code they care about, in small reviewed increments; set up CLAUDE.md or AGENTS.md |
| `karpathy-vibe-to-agentic` | turn a quick prototype into something others can rely on |
| `karpathy-minimalism` | decide whether a dependency is needed, cut dependencies, or design a tool that agents can use |
| `karpathy-supply-chain-hygiene` | audit, pin or safely install dependencies; respond to a compromised package |
| `karpathy-autoresearch` | run an autonomous experiment loop against a metric a script can compute |
| `karpathy-llm-wiki` | build, feed, query or health-check a Markdown knowledge base |
| `karpathy-output-evolution` | present a result as HTML, slides or another format suited to its reader |
| `karpathy-education-first` | teach something: a tutorial, a minimal runnable example, an explanation |
| `karpathy-llm-simulator` | stress-test a decision, plan or draft by having opposing views argued |
| `karpathy-understanding-first` | check what in AI-produced work was verified and what they must look at themselves |
| `karpathy-system-prompt-learning` | stop an agent repeating a mistake by writing the lesson into its instructions |
| `karpathy-practice-environments` | build tasks with automatic scoring for an agent or a person to practise on |
| `karpathy-meta-reflection` | review which of their own skills AI is eroding and which it is amplifying |

## Output

One line naming the chosen skill or workflow and the reason, followed by the output of the loaded skill. After a workflow, add a short summary: each step, what it produced, and any step that was skipped.

## Done when

- [ ] One skill, one workflow, or two skills for a two-part request were chosen and named to the user; or the user was asked the single clarifying question; or the user was told that no skill fits.
- [ ] Every skill used was loaded in full, not applied from its table summary.
- [ ] Each loaded skill's Done when list holds, or the user chose to skip it.

## Workflow

The four workflows are defined here and nowhere else. Names in the chains omit the `karpathy-` prefix. A typical request: `Use the Karpathy "Idea to Launch" workflow to ...`

| Workflow | Chain and what each step does |
|---|---|
| **Idea to Launch** | `idea-files` (write the idea and its success criteria) → `agentic-engineering` (build the first working version in reviewed increments) → `minimalism` (review every dependency the build pulled in) → `supply-chain-hygiene` (audit and pin what remains) → `vibe-to-agentic` (harden against explicit acceptance criteria) |
| **Research to Publication** | `autoresearch` (run the experiments) → `llm-wiki` (file the findings) → `output-evolution` (render them for the audience) → `education-first` (write the teaching version) |
| **Debiased Decision** | `llm-simulator` (argue the sides) → `understanding-first` (separate verified from assumed) → `system-prompt-learning` (record the reusable rule) |
| **Monthly Check-up** | `meta-reflection` (audit the month) → `understanding-first` (find recent work the user cannot explain) → `practice-environments` (set up practice for one skill) → `education-first` (write up what was learned) |

Rules that override the listed order:

- **Idea to Launch.** When a new dependency is proposed during the build, load `minimalism` and `supply-chain-hygiene` at that moment; this is the exception to loading in turn. In the last step the idea file's success criteria are the acceptance criteria.
- **Research to Publication.** `autoresearch` needs a metric a script can compute. For research without one, such as a literature review, skip it and start at `llm-wiki`.
- **Debiased Decision.** The last step runs only if the user has decided and wants a reusable rule. Never record a rule that rests on an item still marked Not verified.
- **Monthly Check-up.** The user supplies the tasks and the recent work to examine. Each skill's Workflow section says what it does differently in this chain.

## Source

- The dated list of posts behind every skill is in `references/index.md`, next to this file.
- The idea that ties the pack together, "you can outsource your thinking but you cannot outsource your understanding", was written by @yacineMTB. Karpathy reposted it on 2026-04-30 as "the quote I've been citing a lot recently": https://x.com/karpathy/status/2049907410303865030

The procedure, templates and thresholds here are this repository's elaboration.
