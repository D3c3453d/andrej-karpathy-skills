---
name: karpathy-education-first
description: 'Turn a project or concept into something that teaches: a minimal runnable example, a from-scratch explanation, a tutorial or a blog post. Use this skill when the user wants to explain a project to beginners, create a tutorial from code, write documentation that teaches, make a concept accessible, or says "make this teachable", "explain like im a beginner", "nano project version", "teaching version", "explain from scratch", "blog post about this".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-output-evolution
  - karpathy-practice-environments
  - karpathy-llm-wiki
  - karpathy-autoresearch
---

# Education First

Reduce a system to its smallest complete form: minimal code that runs, with an explanation of why each part is there. That is how others learn it, and how the author finds out whether they understand it.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- The user wants a teaching version of something they built.
- The user wants a concept explained from scratch.
- The user wants a tutorial, a blog post, or documentation that teaches.

## When not to use

- Reference documentation that should stay terse.
- Production code. Do not rewrite it into teaching code; write the teaching version beside it.
- The user wants only the answer.

## Inputs

- **The project or concept** (required).
- **The audience and what they already know.** If the user does not say, assume a reader who can program but is new to the topic, and state that assumption.
- **The form**: nano-project, explanation, teaching documentation or blog post. Choose from the request.

## Procedure

1. Write the core idea in one sentence. If you cannot, you do not yet understand the material well enough: read more or ask the user.
2. Produce the chosen form, following its rules below.
3. Check the result against Done when.

### Nano-project

A single file that demonstrates one concept.

- Under about 300 lines; standard library only, unless the concept is itself a library.
- Complete: it runs from scratch and prints something meaningful.
- The structure of the code mirrors the structure of the concept.
- Comments explain why, not what.
- The file opens with a three-sentence explanation and the command to run it.
- **Run it before delivering** and include the real output. If you cannot run it, say so.
- Say what was left out for clarity: efficiency, error handling, edge cases.
- End with three to five exercises, easy to hard.

### Explanation

1. The intuition in one sentence: what the thing is.
2. The smallest concrete example, with real numbers or code.
3. Why it matters, in one or two sentences, without hype.
4. The most common misconception, and how a reader can tell whether they hold it.
5. Its limits: what it does not do, or what it assumes.
6. Where to go deeper: only resources you have confirmed exist in this session. Otherwise leave this out.

For a research paper, add: what the paper figured out, how, in plain language, and an idea for a nano-project that would teach it.

### Teaching documentation

Rewrite reference documentation so that it teaches, without changing what it says the software does.

- State the learning goal at the top: `After reading this you can [...]`.
- Lead with a working example, not with definitions.
- For each parameter: what it does, when to change it, the default.
- Add up to three common patterns and up to three common mistakes that the documentation, code, tests or issues actually show. Omit either list if there is nothing real to put in it.

### Blog post

Structure: why this matters now; the one insight; a working example; what did not work; three to five takeaways; what to explore next.

- Be direct and concrete. Prefer exact figures to adjectives.
- Use only figures, errors and failed attempts that the user provided or that you produced in this session. Never invent a benchmark or an anecdote.
- Write in the user's voice. Do not write as if Karpathy were the author.
- End when the content ends.

## Output

The teaching artefact in the chosen form, saved where the user says or shown in the reply. For a nano-project: the file, the command to run it, and its real output.

## Done when

- [ ] The core idea is stated in one sentence.
- [ ] Any example code was run and its real output is shown, or the reply says it was not run.
- [ ] The explanation says why, not only what.
- [ ] (nano-project) Exercises get harder step by step.
- [ ] (explanation) The common misconception comes with a way to check for it.
- [ ] A reader at the stated level can follow the first step without outside help.
- [ ] No resource, figure or anecdote was invented.

## Workflow

The last step of two workflows. In **Research to Publication** it turns the rendered findings from `karpathy-output-evolution` into a teaching version; use the Blog post form unless the user names another. In **Monthly Check-up**, ask the user to explain what they practised in `karpathy-practice-environments`, write it up in the Explanation form from their words, and mark the gaps; explaining it is the test of having learned it.

## Source

- Karpathy's microgpt, 2026-02-11: "Train and inference GPT in 243 lines of pure, dependency-free Python." He adds: "Everything else is just for efficiency." He called it an art project; treating it as a model for teaching, and the 300-line guideline, are this repository's reading. https://x.com/karpathy/status/2021694437152157847
- On joining Anthropic, 2026-05-19: "I remain deeply passionate about education and plan to resume my work on it in time." https://x.com/karpathy/status/2056753169888334312

The procedure, templates and thresholds here are this repository's elaboration.
