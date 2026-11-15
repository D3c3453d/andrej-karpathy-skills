---
name: karpathy-practice-environments
description: 'Build a practice environment: a set of tasks with automatic scoring and feedback, where an agent can be evaluated or a person can practise a skill. Use this skill when the user wants automated evaluation for an agent, a sandbox for testing LLM capabilities, a practice gym for a skill, a curriculum, or says "eval environment", "practice gym", "agent sandbox", "automated testing for agents", "RL environment", "agent eval loop".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-education-first
  - karpathy-understanding-first
  - karpathy-meta-reflection
  - karpathy-autoresearch
---

# Practice Environments

Skills are learned by practice with feedback, not by reading alone. A practice environment is a set of tasks with automatic scoring and feedback. It can be used to evaluate an agent, to find where an agent fails, or to give a person deliberate practice.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- The user wants an agent's ability at something measured before trusting it.
- The user wants a repeatable evaluation set for an agent or a prompt.
- The user wants practice exercises with feedback, for themselves or for others.

## When not to use

- Tasks with no checkable answer, such as open creative work. Use human review.
- A single question. Just answer it.

## Inputs

- **The skill**, stated as observable behaviour (required). `Writes SQL that returns the correct rows for a described question` is usable; `better at SQL` is not. Ask until it is specific.
- **The learner**: an agent or a person.
- **What counts as correct**, and whether a script can check it.
- **Current and target level**, if known.

## Procedure

1. **Write the three kinds of material** a textbook has:
   - Exposition: what the skill is and the concepts behind it, in a few sentences.
   - Worked examples: two or three solved tasks with the reasoning shown, including one edge case.
   - Tasks with known answers or checks: at least five practice tasks and at least five held-back tasks, ordered from easy to hard and varied enough that memorising one answer does not help.
2. **Build the scorer.** Prefer, in this order: a functional check (run the answer and compare results), an exact match, a rubric. A rubric applied by a model is not ground truth: use it only where nothing better exists, and have a person spot-check it.
3. **Define the feedback** given after each attempt: the score, what was wrong, and one hint. Do not give away the solution while retries remain.
4. **Hold tasks back.** The held-back tasks stay out of the practice loop and are used only for the final test. Passing tasks that were seen many times proves little.
5. **Agree the pass mark** with the user before any run. Default: 80% of the held-back tasks.
6. **Isolate it.** Run attempts in a temporary directory with no credentials in the environment, or ask the user for a sandbox.
7. **Run the loop**: attempt, score, feedback, retry. Limit retries per task (three is a reasonable default) and record every attempt.
   - For an agent learner, ask the user for the command that runs that agent in a fresh session, without the answers.
   - Never attempt tasks you wrote yourself and report the result as a measurement.
   - If you cannot run the learner, deliver the environment and say that no run took place.
8. **Make the learning last.** An agent keeps nothing from one session to the next. After the run, write the recurring mistakes into its instructions with `karpathy-system-prompt-learning`, then rerun on the held-back tasks. For a person, list what to practise next.

### Minimal gym (scoring core)

This is only the scoring core. It has no held-back set, retry limit or feedback, and `pass_rate` counts every attempt, retries included. Add those before using it as an environment.

```python
"""Minimal practice gym: generate tasks, score attempts, track progress."""
from typing import Any, Callable

PASS_SCORE = 0.8


class PracticeGym:
    def __init__(self, task_generator: Callable[[], Any], scorer: Callable[[Any, Any], float]):
        self.task_generator = task_generator  # returns a new task
        self.scorer = scorer                  # (task, attempt) -> score from 0.0 to 1.0
        self.history: list[dict] = []

    def sample_task(self) -> Any:
        return self.task_generator()

    def evaluate(self, task: Any, attempt: Any) -> dict:
        score = self.scorer(task, attempt)
        result = {"task": task, "attempt": attempt, "score": score, "passed": score >= PASS_SCORE}
        self.history.append(result)
        return result

    def summary(self) -> dict:
        scores = [r["score"] for r in self.history]
        n = len(scores)
        if n == 0:
            return {"total_attempts": 0, "avg_score": None, "pass_rate": None, "trend": "no data"}
        half = n // 2
        early, late = sum(scores[:half]), sum(scores[n - half:])  # equal-sized halves
        trend = "improving" if late > early else "declining" if late < early else "flat"
        return {
            "total_attempts": n,
            "avg_score": sum(scores) / n,
            "pass_rate": sum(s >= PASS_SCORE for s in scores) / n,
            "trend": trend,
        }
```

### Example rubric, for code generation

Use a rubric like this only where a functional check is not possible.

| Score | Meaning |
|---|---|
| 1.0 | Correct output, edge cases handled |
| 0.8 | Correct output, one edge case missed |
| 0.5 | Core logic right, fails on some inputs |
| 0.2 | Wrong approach, partly useful |
| 0.0 | Does not run, or plainly wrong |

## Output

- The environment: exposition, worked examples, practice tasks, held-back tasks, scorer, feedback format.
- If a run took place: the score per task, the pass rate on held-back tasks, the most common failure, and the lessons written down.

## Done when

- [ ] The skill is defined as observable behaviour.
- [ ] The worked examples cover a typical case and an edge case.
- [ ] Scoring needs no human judgment, or the reply says which part does.
- [ ] Feedback says what was wrong, not only that something was.
- [ ] Tasks rise in difficulty, and some were held back for the final test.
- [ ] Lessons from the run were written down where the learner will see them again, or the reply says that no run took place.

## Workflow

Step 3 of **Monthly Check-up**. Here the learner is the user: it takes the weakest area found by `karpathy-understanding-first`, builds practice for that one skill, and hands what the user practised to `karpathy-education-first` to write up.

## Source

- Karpathy, 2025-01-30: "We have to take the LLMs to school." A textbook has exposition, worked problems and practice problems, matching pretraining, supervised finetuning and reinforcement learning. "They have to read, and they have to practice." https://x.com/karpathy/status/1885026028428681698
- Karpathy, 2025-08-27: environments let a model act and see outcomes, and serve both training and evaluation; what is needed is "a large, diverse, high quality set of environments, as exercises for the LLM to practice against". He adds a caveat: "I am bullish on environments and agentic interactions but I am bearish on reinforcement learning specifically", and names system prompt learning as one alternative. https://x.com/karpathy/status/1960803117689397543

Both posts are about models. Applying the idea to a person's practice is this repository's adaptation, as are the pass mark, the retry limit and the held-back set.

The procedure, templates and thresholds here are this repository's elaboration.
