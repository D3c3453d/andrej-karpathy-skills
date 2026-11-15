---
name: karpathy-agentic-engineering
description: 'Apply Karpathy-style agentic engineering to a coding or building task: agree success criteria, discuss approaches before code, change one thing at a time and verify it. Use this skill when the user wants to build or change code they care about with an agent, needs to write a CLAUDE.md or AGENTS.md, wants an agent workflow with clear success criteria, or says things like "agent loop", "let the agent handle it", "agentic coding", "set up agent workflow".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-idea-files
  - karpathy-minimalism
  - karpathy-supply-chain-hygiene
  - karpathy-vibe-to-agentic
  - karpathy-understanding-first
---

# Agentic Engineering

Build software by working in small, verified increments: agree what done means, discuss approaches before writing code, change one thing, prove it works, then move on. The human supplies direction, taste and review. The agent supplies execution and verification.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- A coding task whose result the user intends to keep.
- Setting up a project to be worked on with an agent, including writing `CLAUDE.md` or `AGENTS.md`.
- Debugging, where each hypothesis can be tested.

## When not to use

- A throwaway prototype or exploration. Just build it; use `karpathy-vibe-to-agentic` later if it needs to last.
- A trivial edit. Make the change and say what you did.
- A decision that is not about code. Use `karpathy-llm-simulator`.

## Inputs

- **Task** (required): what should be true when the work is done.
- **Success criteria** (required): checks that can be run or observed. If the user gave none, propose two to four and get them confirmed before writing code.
- **Project context**: read the relevant files yourself. Find out how to run the tests and the linter. If there is no test command, say so and agree how each change will be verified.
- **Constraints**: areas not to touch, and the policy on new dependencies.

## Procedure

Repeat this loop once per increment.

1. **Gather context.** Read everything relevant to the next change: the files it touches, their callers, their tests, recent diffs. Read the whole project only if it is small.
2. **State the next single increment** and how it will be checked. Turn vague goals into checks: `fix the bug` becomes `a test that reproduces the bug passes`.
3. **Propose before coding.** Where more than one reasonable approach exists, give two or three with their trade-offs and recommend one; otherwise state the approach. List the assumptions you are making. If the request is ambiguous, or a simpler way exists, say so now. Wait for the user's choice, unless they told you to proceed on your own judgment.
4. **Implement the smallest version** of the chosen approach. Change only what this increment needs. Where a test can express the check, write the test first. For performance work, write the obviously correct version first, then optimise while the tests keep passing.
5. **Review.** Show the diff, or a summary if it is long. Explain any API, library call or construct the user may not know. Check each unfamiliar API against the installed version's documentation or source; if you cannot check it, label it UNVERIFIED.
6. **Verify.** Run the tests, the linter and the success criteria, and report the real output. If something fails, fix it, or reverse your own edits for this increment only and try another approach. Never use `git checkout --`, `git restore`, `git reset --hard` or `git stash` on files that hold uncommitted work. Never weaken a check to make it pass.
7. **Commit** only if the user asked for commits in this session; otherwise leave the change for them. Then, if every success criterion passes, stop and report; do not start increments outside the task. Otherwise suggest the next increment and return to step 1.

### Failure modes to guard against

Karpathy lists these as the common mistakes of coding agents (2026-01-26).

| Failure | Guard |
|---|---|
| Acting on an unstated assumption | List assumptions in step 3; ask when a wrong guess would be costly |
| Hiding confusion instead of asking | Name what is unclear and ask |
| Agreeing when pushback is due | Say so when a request looks wrong or a simpler way exists |
| Over-complicated code and needless abstractions | Write the minimum; no options or layers nobody asked for |
| Dead code left behind | Remove what your change made unused; mention older dead code, do not delete it |
| Edits to code or comments unrelated to the task | Every changed line must trace to the current increment |

This repository adds one more guard: never add a dependency without asking, and apply `karpathy-minimalism` and `karpathy-supply-chain-hygiene` first.

### Project instruction file

When asked to set up a project for agent work, put this in the instruction file your host reads at the repository root (`CLAUDE.md` for Claude Code, `AGENTS.md` for most others; ask if unsure), filled in from the project. If the file already exists, read it and propose an added section; never replace it. Instructions reduce the failures above but do not remove them (Karpathy reports they persisted despite his own), so step 5 still applies.

```markdown
# Agent instructions for [project name]

## Rules
- One incremental change at a time. Do not refactor unrelated code.
- Before a non-trivial change, state your plan and assumptions.
- If the intent is unclear, ask before implementing.
- No new dependencies without approval.
- Prefer simple, readable solutions.

## Project
- Stack: [language, framework, key libraries]
- Entry point: [file]
- Run tests: `[command]`
- Run linter: `[command]`
- Never: [things the agent must not do]
```

## Output

After each increment, report:

- **Changed:** each file and one line on what changed.
- **Verified:** the commands run and their results.
- **Not verified:** assumptions and anything marked UNVERIFIED.
- **Next:** the suggested next increment.

## Done when

- [ ] Success criteria were written as checks before any code.
- [ ] Every success criterion passes, with its output shown.
- [ ] Every increment has real verification output, not a claim that it should work.
- [ ] No file outside the task was changed.
- [ ] Each diff was shown, and unfamiliar parts were explained.
- [ ] The whole change can be stated in one sentence: what changed and why.

## Workflow

Step 2 of **Idea to Launch**. It takes the idea file from `karpathy-idea-files` as its task and success criteria, and hands the working code to `karpathy-minimalism`. It also works on its own.

## Source

- The loop is adapted from Karpathy's rhythm for code he professionally cares about, posted 2025-04-25: "Stuff everything relevant into context", describe the next single change, ask for approaches before code, review and learn, test, commit. He stresses "keeping a very tight leash" and being "slow, defensive, careful, paranoid". In his loop the commit is unconditional; making it depend on the user's wishes is this repository's change. https://x.com/karpathy/status/1915581920022585597
- The failure modes and the advice on leverage are from his notes of 2026-01-26: "Don't tell it what to do, give it success criteria and watch it go." https://x.com/karpathy/status/2015883857489522876
- On 2026-02-25 he wrote that agents do best on "tasks that are well-specified and where you can verify/test functionality". https://x.com/karpathy/status/2026731645169185220

The procedure, templates and thresholds here are this repository's elaboration.
