---
name: karpathy-meta-reflection
description: 'Run a structured, evidence-based review of how AI is changing the user''s own skills: which are fading, which new abilities have appeared, and what to practise, learn or hand over next. Use this skill for monthly career audits, skill atrophy reviews, deciding what is worth learning versus delegating to AI, or when the user says "career audit", "skills review", "what should I learn", "am I becoming obsolete", "AI impact on my work", "leverage review", "meta reflection".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-understanding-first
  - karpathy-practice-environments
  - karpathy-education-first
  - karpathy-system-prompt-learning
---

# Meta-Reflection

When AI takes over parts of someone's work, some of their skills fade and some new abilities appear. A periodic review based on what they actually did lets them choose which skills to keep sharp, which to hand over and what to learn, instead of finding out by accident.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- A monthly or other periodic review of the user's work with AI.
- The user asks what to learn, what to delegate, or whether their skills are slipping.
- The user is deciding whether one particular skill is worth learning deeply.

## When not to use

- The user wants to talk through a worry, not to be audited. Listen first; offer the review, do not impose it.
- Evaluating other people's performance.

## Inputs

- **Role or field** (required).
- **Period** under review; the default is the last month.
- **The tasks the user actually did** in that period, with the AI-assisted ones marked (required). They come from the user, or from records the user points you to, such as a git log, a calendar or notes. Never invent them. If the user cannot list them, help them reconstruct the list with questions.

## Procedure

### Monthly review

1. **Collect** 5 to 15 real tasks from the period.
2. **For each AI-assisted task**, ask and record:
   - Could you still do this unaided? Has that changed?
   - Could you still review the result and spot a mistake? Reviewing and producing are different abilities.
   - What would have happened if the AI's output had been wrong and you had not noticed?
3. **Classify** the skill behind each task. Propose a class with your reason; the user decides.

   | Class | Test | Action |
   |---|---|---|
   | KEEP SHARP | You must be able to judge whether the AI's output is right, or mistakes are costly | Keep practising at the level that judging needs: careful review at least, unaided work where review alone is not enough |
   | HAND OVER | Routine, easy to check, cheap if wrong | Delegate it fully and stop spending practice time on it |
   | BUILD | A new ability that AI makes possible, or much faster, for you | Invest in the tools and the workflow |

4. **Use evidence, not estimates.** Record the user's own observations, for example `two hours last year, twenty minutes now`. Do not supply percentages, or claims about what AI can do in their field, unless the user gave them or you verified them in this session.
5. **Role statement.** Have the user complete: `A year ago my job was mainly [X]. Now it is mainly [Y].`
6. **Plan for the next period**, exactly three items, each concrete enough to schedule:
   - one skill to practise without AI: what, when, how often;
   - one AI capability to learn;
   - one task to hand over fully.
7. **Save** the report as `ai-reviews/[YYYY-MM-DD].md` in a folder the user names; ask once and tell them the path. Look there for the previous report and note what changed. If none is found, write `first review`.

### Weekly check-in (a five-minute version)

```text
Week of [date]
Three things AI helped me do better:
One thing I learned from reviewing AI output:
One thing I did without AI:
One thing to watch next week:
```

### Learn-or-delegate test for one skill

Ask the user these four questions and record their answers with reasons.

1. Does AI already do this at a quality that is acceptable for your use? Base the answer on their experience or a quick trial, not on an assumption.
2. Is human judgment still needed for the part that matters?
3. Would understanding it deeply make you better at directing and checking AI on it?
4. Is it a foundation for judgment that you rely on elsewhere?

Recommend by the first rule that matches:

1. Question 1 is no: the work is still theirs to do. Learn it as far as the work requires.
2. Question 3 or 4 is yes: LEARN DEEPLY.
3. Question 2 is yes: LEARN THE BASICS, enough to check the AI's output.
4. Otherwise: DELEGATE.

## Output

```markdown
# AI impact review: [period]
Role: [role]

## Tasks reviewed
| Task | AI-assisted | Class | Evidence |

## Keep sharp
## Hand over
## Build
## Role statement
## Plan for next period
1. Practise without AI: [...]
2. Learn: [...]
3. Hand over: [...]

## Changes since the last review
```

## Done when

- [ ] Every task in the review is one the user actually did.
- [ ] Each fading skill is backed by a specific observation, not general worry.
- [ ] Each gain is backed by the user's own evidence.
- [ ] The practice item says what, when and how often.
- [ ] The hand-over item names one specific task.
- [ ] The report is dated and saved where the next review can find it.

## Workflow

Step 1 of **Monthly Check-up**. Its KEEP SHARP list goes to `karpathy-understanding-first`, which checks how well the user can still explain recent work in those areas.

## Source

- Karpathy, 2025-12-26: "I've never felt this much behind as a programmer." He describes a new layer of agents, prompts, tools and workflows to master, and says that "a failure to claim the boost feels decidedly like skill issue". https://x.com/karpathy/status/2004607146781278521
- Karpathy, 2026-01-26: he reports that his ability to write code by hand is starting to atrophy, and notes that "Generation (writing code) and discrimination (reading code) are different capabilities in the brain", so that "you can review code just fine even if you struggle to write it". He dates a phase shift in software engineering to around December 2025. https://x.com/karpathy/status/2015883857489522876

Karpathy reported these observations about himself. The periodic review, the three classes and the test are this repository's own.

The procedure, templates and thresholds here are this repository's elaboration.
