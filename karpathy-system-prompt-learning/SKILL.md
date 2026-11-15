---
name: karpathy-system-prompt-learning
description: 'Turn a mistake or a hard-won insight into an explicit, reusable strategy written into the instructions an agent reads next time: a system prompt, CLAUDE.md, AGENTS.md or a skill file. Use this skill when an agent keeps repeating a mistake, the user wants a correction to stick, a system prompt is not working, or the user says "improve system prompt", "encode this strategy", "prompt engineering", "make agent follow this workflow", "prompt as textbook".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-understanding-first
  - karpathy-llm-simulator
  - karpathy-agentic-engineering
  - karpathy-practice-environments
---

# System Prompt Learning

When an agent hits a problem and a better approach is worked out, write the lesson down as an explicit strategy in the instructions the agent will read next time. A lesson that lives only in one conversation is gone in the next.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- An agent made the same kind of mistake more than once.
- The user corrected an approach and wants the correction to last.
- A decision or debugging session produced a rule worth reusing.
- An existing system prompt or instruction file is not producing the wanted behaviour.
- A prompt for a new agent has to be written.

## When not to use

- A preference that matters only in this conversation.
- A rule that must hold every single time. Enforce it in code instead: a test, a linter rule, a hook.
- Facts about the user. This skill records general strategies for solving problems, not personal details.

## Inputs

- **The failure or the lesson**, with at least one concrete example of what happened (required for part A).
- **Where the instructions live**: `CLAUDE.md`, `AGENTS.md`, a skill file, a system prompt. Ask if unknown.
- **The current text** of those instructions. Read it yourself.

## Procedure

### A. Capture a lesson

1. **Describe the failure concretely**: the situation that triggers it, what was done, what should have been done. Work from the real example; do not generalise from nothing.
2. **Find the cause** by reading the existing instructions: missing knowledge, a missing rule, two instructions in conflict, or ambiguous wording. If a rule already covers the case and was ignored, rewrite or move that rule; do not add a second one.
3. **Write the lesson** in under about 200 words:

   ```text
   When [specific trigger], do [specific action], because [reason].
   Not: [the mistake to avoid].
   Example: [short before -> after]
   ```

4. **Check for conflicts** with the existing instructions and resolve them. Never stack contradictory rules.
5. **Propose the edit**: show the exact text and the file. Write it once the user approves, or straight away if they already asked you to write it. These files change how agents behave in future sessions.
6. **Test it** in a fresh session or subagent that has only the edited instructions, on the failing case or a similar new one. Applying the lesson yourself in this conversation is not a test, because the correction is already in your context. If you cannot run such a test, say the lesson is untested.
7. **Keep the file healthy.** A long list of rules gets ignored. If you see duplicates or lessons that look obsolete, list them and propose the merge or deletion alongside the edit in step 5. Change nothing beyond what the user approved.

Example lesson:

```text
When a test fails after your change, read the full error and the test before
editing anything, because the first guess is often aimed at the wrong file.
Not: changing the assertion so that the test passes.
Example: "expected 3, got 4" -> found an off-by-one in the loop bound, fixed
the loop, left the test alone.
```

### B. Write or repair a whole prompt

Inputs for this part: the agent's task, and real examples of good and bad output from the user. Do not invent domain facts or examples; ask for them.

Give the prompt three kinds of material, adapted from the textbook analogy (strategies stand in for practice problems):

- **Background**: what the model needs to know and does not: the domain, its vocabulary, the context.
- **Worked examples**: one to three complete examples with the reasoning shown, including one hard case.
- **Strategies**: explicit `when / do / because` rules, the mistakes to avoid, and a short checklist to run before answering.

To repair a failing prompt:

1. Collect one bad output and one good output for the same input.
2. Decide which of the three parts is missing or wrong.
3. Change one thing.
4. Retest on the same cases and on one new case.
5. Keep a version note: what changed, why, and the measured result. Record only results you measured.

## Output

- The lesson or the revised prompt, as exact text.
- The file it belongs in, and whether it has been written or is waiting for approval.
- The test result, or a statement that it was not tested.

## Done when

- [ ] The trigger is specific; it does not say `sometimes`.
- [ ] The action is concrete enough to follow without interpretation.
- [ ] The mistake to avoid is named.
- [ ] There is a before and after example.
- [ ] The lesson is under about 200 words and conflicts with no existing instruction.
- [ ] It was tested in a fresh session, or the reply says it is untested.
- [ ] For a whole prompt (part B): one change per revision, retested on the same cases plus one new one, with only measured results recorded.

## Workflow

Step 3, the last, of **Debiased Decision**. It runs only when the user has made the decision and wants a reusable rule: `When [situation], do [choice], because [verified reason]`. Never record a rule that rests on an item `karpathy-understanding-first` left under Not verified. If there is nothing to record, say so in one line and end. It is also used on its own whenever an agent repeats a mistake.

## Source

Karpathy's post of 2025-05-11, proposing system prompt learning as a missing way for LLMs to learn: https://x.com/karpathy/status/1921368644069765486

- "Pretraining is for knowledge." "Finetuning (SL/RL) is for habitual behavior." Both change the model's weights.
- Much human learning is closer to writing a note: "It feels more like taking notes for yourself".
- He argues these strategies should not be handwritten by engineers; the model should accumulate them from experience, "the LLM writing a book for itself on how to solve problems". Part A follows that idea.
- On 2025-08-27 he described the idea again as "moving the update to tokens/contexts not weights". https://x.com/karpathy/status/1960803117689397543
- The three-part structure in part B is adapted from his textbook analogy of 2025-01-30 (exposition, worked problems, practice problems), which he drew for training data, not for prompts. https://x.com/karpathy/status/1885026028428681698

The procedure, templates and thresholds here are this repository's elaboration.
