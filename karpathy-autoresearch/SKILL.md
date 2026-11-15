---
name: karpathy-autoresearch
description: 'Set up and run an autonomous experiment loop: the human writes the instructions and fixes the metric, the agent changes code, runs it, keeps what improves the metric and repeats without asking. Use this skill when the user wants to automate ML experiments, run many variations of an experiment unattended, optimise a prompt or a benchmark against a measurable score, or says "automate experiments", "agent research loop", "run hyperparameter search", "autoresearch".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-llm-wiki
  - karpathy-output-evolution
  - karpathy-education-first
  - karpathy-system-prompt-learning
---

# AutoResearch

An autonomous experiment loop. The human writes the instructions and fixes how success is measured. The agent changes the code, runs it, measures, keeps what improved the metric, discards what did not, and repeats without stopping to ask.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

The goal can be expressed as one number that a script computes in bounded time:

| Domain | Metric |
|---|---|
| ML training | validation loss after a fixed training budget |
| Prompt optimisation | pass rate on a fixed evaluation set |
| Code performance | benchmark runtime, with the tests still passing |

## When not to use

- No script can compute the metric: literature reviews, open exploration, anything needing real users (click rates, usability). Use `karpathy-llm-wiki` to organise that kind of research.
- A run costs real money or touches production, and no limit has been agreed.
- The agent could raise the score by editing the evaluation. Separate and protect the evaluation first.

## Inputs

All are required before the loop starts. Ask for what is missing.

- **Run command**: the command that runs one experiment and prints the metric.
- **Metric**: one number, and whether lower or higher is better.
- **Editable scope**: the files you may change. Everything else is read-only, above all the evaluation code and its data.
- **Run budget**: a fixed time or compute limit per run, so that runs are comparable.
- **Stop limits**: maximum number of experiments, total time or total cost; or explicitly `until interrupted`.
- **Ideas** to try or to avoid (optional).

## Procedure

### Setup, with the user

1. Before writing anything, check that no tracked file has uncommitted changes: `git status --porcelain --untracked-files=no` must print nothing. If it prints something, stop and ask: the loop discards such changes.
2. Confirm the inputs and draft `program.md` from them. From here on the file belongs to the human: reread it at the start of every experiment and never edit it.
3. Create a branch `autoresearch/[tag]` from the current branch, where `[tag]` is today's date (for example `mar5`) unless the user names one. Never run the loop on the main branch.
4. Create `results.tsv` with the header `commit	metric	status	description` (tab-separated). Add `program.md`, `results.tsv` and `run.log` to `.git/info/exclude`, so that they are never committed and never reset.
5. Run the unmodified code to record the baseline. Unless the run is deterministic, run it three times; the spread between those runs is the noise.
6. Show the user the setup and the baseline. Start the loop when they confirm.

### The loop, without check-ins

1. Reread `program.md`. Choose one idea and change only files in scope. By default make one change per experiment, so the result can be attributed to it. If you run out of ideas, reread the in-scope files, combine earlier near-misses, or try a more radical change.
2. Stage only the in-scope files, by path, and commit: `git add -- [scope files]`, then `git commit`. Never use `git add -A`, `git commit -a`, `git stash` or `git clean`.
3. Run with all output sent to a file: `[run command] > run.log 2>&1`. If the command does not stop itself at the budget, wrap it in `timeout`; a run that times out is a crash. Read only the metric line; on failure read the last 50 lines of the log.
4. Add a row to `results.tsv` with status `keep`, `discard` or `crash`.
5. Keep the commit if the metric improved by more than the noise, or if it is within the noise and the code got simpler. Otherwise discard it:
   - run `git status --porcelain --untracked-files=no`; if it shows a change to a tracked file outside the scope, stop and ask, because someone else is editing;
   - run `git reset --hard [last kept commit]`;
   - delete only the untracked files that this experiment created.
6. On a crash: fix a trivial mistake and rerun; if the idea itself is broken, record `crash`, discard as in step 5 and move on.
7. Go to step 1.

A worse result is normal and is never a reason to stop: most experiments fail. Do not pause to ask whether to continue. Stop only when:

- a stop limit is reached or the user interrupts;
- three runs in a row crash and you cannot fix the cause;
- continuing would break a rule below.

### Rules

- Never modify the evaluation, the metric code or the data.
- Never add dependencies.
- Never touch files outside the scope, other branches, or the remote.
- Never exceed the stop limits.

### When the loop ends

Report the best result and its commit, the table of experiments, what helped and what did not, and what to try next. Add the caveat that every improvement was measured on one metric under one budget.

## Output

- The branch `autoresearch/[tag]`, whose tip is the best version found.
- `results.tsv` with one row per experiment, failures included.
- The final report described above.

## Done when

- [ ] The metric is one number, with a command that prints it.
- [ ] A baseline was recorded before any change.
- [ ] Every experiment has a row in `results.tsv`.
- [ ] All work happened on the experiment branch; the evaluation was not modified.
- [ ] The loop ended for one of the listed reasons, and the report says which.

## Workflow

Step 1 of **Research to Publication**. Its report and `results.tsv` go to `karpathy-llm-wiki` to be filed. If the research has no computable metric, this step is skipped.

## Source

- Karpathy's post of 2026-03-07 announcing the autoresearch repository: "the human iterates on the prompt (.md)" and "the AI agent iterates on the training code (.py)". Each run lasts exactly five minutes, and the agent works on a git feature branch. https://x.com/karpathy/status/2030371219518931079
- The loop follows the repository's `program.md`: a baseline first, a fixed time budget, a read-only evaluation, keep the commit on improvement and reset otherwise, an untracked results file, log output redirected, simpler is better, what to do when ideas run out, and no pausing to ask. https://github.com/karpathy/autoresearch/blob/master/program.md

The original loop has no end; it runs until interrupted. The stop limits, the three-crash stop, the noise check, the one-change default and the git safety steps are this repository's additions.

The procedure, templates and thresholds here are this repository's elaboration.
