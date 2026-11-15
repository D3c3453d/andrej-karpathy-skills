---
name: karpathy-llm-simulator
description: 'Stress-test a decision, plan or draft by having the model voice several conflicting perspectives instead of giving one agreeable answer. Use this skill when the user needs to make an important decision, wants to stress-test an idea, needs devil''s advocate analysis, wants to avoid confirmation bias, or says "argue against this", "what are the counterarguments", "simulate experts", "debate this", "challenge my thinking".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-understanding-first
  - karpathy-system-prompt-learning
  - karpathy-meta-reflection
---

# LLM as Simulator

A language model has no settled opinion of its own, and it can argue almost any side convincingly. Use that: instead of asking what it thinks, have it voice several conflicting perspectives, then let the user judge the arguments. A persuasive argument from a model is not evidence that the argument is true.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- An important decision is about to be made.
- A plan or draft should be stress-tested before the user commits to it or publishes it.
- The user asks for counterarguments, a devil's advocate or a pre-mortem.
- The user wants their own understanding of a concept tested.

## When not to use

- A factual question. Answer it, or look it up.
- Writing code. Use `karpathy-agentic-engineering`.
- The decision is made and the user needs it carried out.
- The decision turns on a fact nobody has checked. Check the fact first; a simulation cannot supply it.

## Inputs

- **The decision, plan or draft**, and the user's current position on it (required).
- **Stakes and constraints**: what it costs to be wrong, what cannot change.
- **A mode**, if the user named one. Otherwise choose from the modes below and say which.

## Procedure

### Rules for every mode

1. **Say that you are simulating.** The personas are not real experts and their statements are not evidence.
2. **No invented facts.** Personas argue from reasoning, from facts the user gave, and from facts verified in this session. No made-up statistics, studies or quotations. If an argument depends on a fact you cannot verify, phrase it as a condition and put the fact on the To verify list.
3. **Do not favour the user.** Judge their position as if it came from a stranger. If it is weak, say so directly.
4. **Argue each side at full strength.** No hedging inside a persona's argument.
5. **Do not force a compromise.** The synthesis reports which arguments survived. It may find that one side is right, that a combination is, or that the question cannot be settled until the listed facts are checked.

### Mode 1: Panel debate

For technical choices, product direction, research direction.

Choose 3 to 5 personas whose interests conflict: at least one who gains if the answer is yes, one who carries the cost or the risk, and one outsider. For each persona give:

- the core argument, in about three sentences;
- the strongest objection to each opposing view;
- what would change this persona's mind.

### Mode 2: Argue the opposite

For the moment before publishing or committing.

Make the strongest possible case against the user's plan or draft. Then state what the original would need in order to survive that case.

### Mode 3: Pre-mortem

For plans and launches.

Assume it is six months later and the plan has failed. Tell three different failure stories, each with its own root cause: a technical failure, a wrong assumption, an execution failure. Describe the sequence of events in each, and name the warning signs that would be visible within the first month.

### Mode 4: Socratic drill

For testing the user's understanding.

Ask five questions of rising difficulty, one at a time, waiting for each answer. After each answer, probe one gap or inconsistency. Finish by naming the largest gap and what to study to close it. For this mode the output is the questions, the gaps probed and the largest gap; the Output template below does not apply.

### Sequence for a full review

Use this sequence only when the user asks for a full review, or says that a wrong decision would be costly or hard to reverse. Otherwise run one mode.

1. Record the user's current position.
2. Run Mode 2.
3. Let the user revise the position.
4. Run Mode 1 on the revised position.
5. The user decides. The decision is theirs, not yours.

## Output

```text
Mode: [mode used]
Positions: [each persona or case, as specified for the mode]
What survived: [arguments still standing, and why]
What did not: [arguments that failed, and what defeated them]
To verify before deciding: [assumption] - [how to check it]
```

## Done when

- [ ] Mode 1: at least three perspectives with conflicting interests. Mode 2: the opposing case at full strength. Mode 3: three failure stories with different root causes. Mode 4: five questions asked one at a time, and the largest gap is named.
- [ ] Each argument is specific to this decision, not generic.
- [ ] No statistic, study or quotation was invented.
- [ ] Modes 1 to 3: the synthesis says what survived and what did not, without bending toward the user's starting position.
- [ ] Modes 1 to 3: the To verify list names each unchecked assumption and how to check it.

## Workflow

Step 1 of **Debiased Decision**. Its To verify list goes to `karpathy-understanding-first`, which separates what has been verified from what is still assumed.

## Source

- Karpathy, 2025-12-07: "Don't think of LLMs as entities but as simulators." Instead of asking for the model's opinion, he suggests asking: "What would be a good group of people to explore xyz? What would they say?" https://x.com/karpathy/status/1997731268969304070
- Karpathy, 2026-03-28: after an LLM helped him polish an argument for four hours, he asked it to argue the opposite and it convinced him of that too. Models are "extremely competent in arguing almost any direction"; his advice is to "make sure to ask different directions and be careful with the sycophancy". https://x.com/karpathy/status/2037921699824607591

The procedure, templates and thresholds here are this repository's elaboration.
