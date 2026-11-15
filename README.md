<div align="center">

# Karpathy-Inspired Skills

#### 14 working methods for AI-assisted work, distilled from Andrej Karpathy's public posts, plus 1 router

[![License](https://img.shields.io/badge/License-MIT-3B82F6?style=for-the-badge)](./LICENSE)
[![Skills](https://img.shields.io/badge/Skills-14%2BRouter-10B981?style=for-the-badge)](#skills)
[![Workflows](https://img.shields.io/badge/Workflows-4-F59E0B?style=for-the-badge)](#workflows)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-8B5CF6?style=for-the-badge)](https://agentskills.io)

**One-line install (Codex, Claude Code, OpenCode, OpenClaw, Hermes)**

```bash
npx skills add D3c3453d/andrej-karpathy-skills
```

**Claude Code plugin** (from a terminal)

```bash
claude plugin install karpathy-skills --marketplace D3c3453d/andrej-karpathy-skills
```

</div>

> **Disclaimer.** This is an unofficial study and reorganisation of public posts by [Andrej Karpathy](https://x.com/karpathy). His posts belong to him. The skills are built around observations in those posts; the procedures are this repository's own. It does not speak for him and he has not endorsed it.

This is not a collection of quotations. Each skill starts from something Karpathy has described and builds a procedure an agent can follow: when to use it, what it needs, what to do, what to produce, and when it is done.

Every skill separates what a cited post actually says from what this repository added. The dated list of sources is in [karpathy-methodology/references/index.md](./karpathy-methodology/references/index.md).

---

## Skills

| Skill | In one line | Main source |
|---|---|---|
| **karpathy-methodology** | Not sure which skill to use? Start here; it picks one and loads it | router |
| **agentic-engineering** | One reviewed, verified increment at a time | [post](https://x.com/karpathy/status/1915581920022585597) |
| **vibe-to-agentic** | Turn a quick prototype into something others can rely on | [post](https://x.com/karpathy/status/1886192184808149383) |
| **autoresearch** | An autonomous experiment loop against a metric a script can compute | [post](https://x.com/karpathy/status/2030371219518931079) |
| **llm-simulator** | Have opposing views argued instead of asking the model what it thinks | [post](https://x.com/karpathy/status/1997731268969304070) |
| **llm-wiki** | A Markdown knowledge base that the LLM writes and maintains | [gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) |
| **minimalism** | Decide whether a dependency is needed at all | [post](https://x.com/karpathy/status/2036487306585268612) |
| **supply-chain-hygiene** | Inspect before installing; pin; respond to a compromised package | [post](https://x.com/karpathy/status/2036487306585268612) |
| **understanding-first** | Separate what was verified from what was assumed in AI-produced work | [post](https://x.com/karpathy/status/2049907410303865030) |
| **idea-files** | Write the idea down as a spec; let an agent build it | [post](https://x.com/karpathy/status/2040470801506541998) |
| **output-evolution** | HTML or slides for people, Markdown or JSON for agents | [post](https://x.com/karpathy/status/2053872850101285137) |
| **education-first** | A minimal runnable example plus an explanation of why | [post](https://x.com/karpathy/status/2021694437152157847) |
| **system-prompt-learning** | Write the lesson from a mistake into the agent's instructions | [post](https://x.com/karpathy/status/1921368644069765486) |
| **practice-environments** | Tasks with automatic scoring, for an agent or a person | [post](https://x.com/karpathy/status/1885026028428681698) |
| **meta-reflection** | Review which of your skills AI is eroding and which it amplifies | [post](https://x.com/karpathy/status/2004607146781278521) |

---

## Workflows

Four chains of skills. They are defined in one place, the router skill ([karpathy-methodology/SKILL.md](./karpathy-methodology/SKILL.md)).

| Workflow | Chain | Example request |
|---|---|---|
| **Idea to Launch** | idea-files → agentic-engineering → minimalism → supply-chain-hygiene → vibe-to-agentic | `Use the Karpathy "Idea to Launch" workflow to take my AI product radar idea to a minimal version that can ship.` |
| **Research to Publication** | autoresearch → llm-wiki → output-evolution → education-first | `Use the Karpathy "Research to Publication" workflow to find the best prompt for our ticket classifier and write up the results.` |
| **Debiased Decision** | llm-simulator → understanding-first → system-prompt-learning | `Use the Karpathy "Debiased Decision" workflow to help me decide whether to install this skill for all agents by default.` |
| **Monthly Check-up** | meta-reflection → understanding-first → practice-environments → education-first | `Use the Karpathy "Monthly Check-up" workflow to review how my coding and writing skills changed this month.` |

Name the workflow in the request. A generic phrase such as `research to publication` on its own may be picked up by some other research or writing skill.

### What each step produces

**Idea to Launch**

| Step | Skill | Produces |
|---|---|---|
| Think it through | idea-files | An idea file with success criteria |
| Build | agentic-engineering | A working version, built in verified increments |
| Slim down | minimalism | A verdict on every dependency: keep, replace, remove |
| Secure | supply-chain-hygiene | An audit of what remains, pinned and locked |
| Harden | vibe-to-agentic | Acceptance criteria met; prototype defects fixed |

**Research to Publication**

| Step | Skill | Produces |
|---|---|---|
| Experiment | autoresearch | A results log and the best version found |
| File | llm-wiki | Wiki pages with sources cited |
| Render | output-evolution | An HTML report or slides |
| Teach | education-first | A minimal example with exercises |

`autoresearch` needs a metric that a script can compute. For research without one, such as a literature review, the workflow starts at `llm-wiki`.

**Debiased Decision**

| Step | Skill | Produces |
|---|---|---|
| Argue | llm-simulator | Opposing cases and a list of assumptions to verify |
| Verify | understanding-first | What is verified, what is not, what to inspect yourself |
| Record | system-prompt-learning | The decision rule, written down for reuse |

**Monthly Check-up**

| Step | Skill | Produces |
|---|---|---|
| Audit | meta-reflection | Skills to keep sharp, hand over, or build |
| Locate | understanding-first | Recent work you cannot explain |
| Practise | practice-environments | Practice tasks with scoring for one skill |
| Write up | education-first | A short tutorial on what you practised |

### Compatibility, as tested upstream

These results come from the upstream author's tests of the original Chinese-language edition on 2026-05-26. **They have not been re-run on this English edition.** Each request ran in a fresh session (`codex exec`, `hermes -z`, `openclaw agent`, `claude -p --no-session-persistence`).

| Workflow | Codex | Hermes | OpenClaw | Claude Code |
|---|---|---|---|---|
| Idea to Launch | pass | pass | pass | pass with an explicit skill chain |
| Research to Publication | pass | pass when the request names Karpathy | pass | pass with an explicit skill chain |
| Debiased Decision | pass | pass | pass | pass with an explicit skill chain |
| Monthly Check-up | partial: no full task list was produced | timeout | pass | pass with an explicit skill chain |

Notes from those tests:

- **Claude Code** did not trigger workflows reliably from a plain-language request; it sometimes ran the wrong chain. Listing the skills explicitly worked in every case (see below).
- **OpenClaw** passed when the four workflows ran one after another. Running all four at once caused timeouts. One run used roughly 63k to 86k tokens.

### How to trigger a workflow

| Agent | How |
|---|---|
| **Claude Code** | List the chain explicitly: `/karpathy-idea-files /karpathy-agentic-engineering /karpathy-minimalism /karpathy-supply-chain-hygiene /karpathy-vibe-to-agentic Use the Karpathy "Idea to Launch" workflow to ...` Putting `/karpathy-methodology` first would also give the agent the workflow's order rules; that variant has not been tested. If the skills were installed as a plugin, each name is `/karpathy-skills:karpathy-...` instead. |
| **Codex** | Open the conversation with the request: `Use the Karpathy "Idea to Launch" workflow to ...` |
| **OpenClaw, OpenCode** | Mention the skill directly, or use the same request as for Codex |
| **Hermes** | Upstream reports that the bundled-skills feature can load a whole chain from its entry skill. Not re-tested here. |

---

## The skills in detail

### karpathy-methodology (router)

Does no task work itself. It matches the request to one skill or one workflow, tells you which, and loads it.

| You are | It loads |
|---|---|
| Writing down an idea | idea-files |
| Building or changing code | agentic-engineering, plus minimalism and supply-chain-hygiene when a dependency is proposed |
| Hardening a prototype | vibe-to-agentic |
| Running experiments against a metric | autoresearch |
| Organising research or notes | llm-wiki |
| Presenting a result | output-evolution |
| Teaching or explaining | education-first |
| Making an important decision | llm-simulator (or the Debiased Decision workflow) |
| Checking AI-produced work | understanding-first |
| Fixing a mistake an agent keeps making | system-prompt-learning |
| Evaluating an agent, or practising a skill | practice-environments |
| Reviewing your own skills | meta-reflection |

→ [SKILL.md](./karpathy-methodology/SKILL.md)

### agentic-engineering

> "Don't tell it what to do, give it success criteria and watch it go." — Andrej Karpathy, 2026-01-26

Agree what done means, discuss approaches before code, change one thing, verify it, repeat. Guards against the agent failures Karpathy lists: unchecked assumptions, no pushback, over-complication, edits outside the task.

**Use for** code you intend to keep. **Not for** throwaway prototypes.
**Workflow:** Idea to Launch.
**Sources:** [coding rhythm](https://x.com/karpathy/status/1915581920022585597), [notes from Claude coding](https://x.com/karpathy/status/2015883857489522876), [programming has changed](https://x.com/karpathy/status/2026731645169185220)

→ [SKILL.md](./karpathy-agentic-engineering/SKILL.md)

### vibe-to-agentic

> "vibe coding raised the floor. Agentic engineering raises the ceiling." — Stephanie Zhan, summarising her talk with Karpathy, quoted by him on 2026-04-30

Audits what a quick prototype actually does, defines acceptance criteria, pins current behaviour with a test, then fixes one issue at a time.

**Use for** a prototype that works but that you would not yet ship. **Not for** code that does not exist yet.
**Workflow:** Idea to Launch.
**Sources:** [vibe coding](https://x.com/karpathy/status/1886192184808149383), [coding rhythm](https://x.com/karpathy/status/1915581920022585597), [notes from Claude coding](https://x.com/karpathy/status/2015883857489522876), [Sequoia Ascent highlights](https://x.com/karpathy/status/2049903821095354523)

→ [SKILL.md](./karpathy-vibe-to-agentic/SKILL.md)

### autoresearch

> "The agent works in an autonomous loop on a git feature branch" — Andrej Karpathy, 2026-03-07

You fix the metric and write the instructions. The agent changes code, runs it, keeps the commit if the metric improved and resets if not, and repeats without asking. Baseline first; the evaluation is read-only.

**Use for** ML training runs, prompt optimisation against a fixed evaluation set, benchmark tuning. **Not for** research without a computable metric.
**Workflow:** Research to Publication.
**Sources:** [autoresearch post](https://x.com/karpathy/status/2030371219518931079), [program.md](https://github.com/karpathy/autoresearch/blob/master/program.md)

→ [SKILL.md](./karpathy-autoresearch/SKILL.md)

### llm-simulator

> "Don't think of LLMs as entities but as simulators." — Andrej Karpathy, 2025-12-07

A model can argue any side convincingly, so a convincing answer proves little. This skill has it voice conflicting perspectives and reports which arguments survive, with a list of assumptions you must check yourself.

| Mode | For |
|---|---|
| Panel debate | Technical and product decisions |
| Argue the opposite | The moment before publishing or committing |
| Pre-mortem | Plans and launches |
| Socratic drill | Testing your own understanding |

**Workflow:** Debiased Decision.
**Sources:** [LLMs as simulators](https://x.com/karpathy/status/1997731268969304070), [argue the opposite](https://x.com/karpathy/status/2037921699824607591)

→ [SKILL.md](./karpathy-llm-simulator/SKILL.md)

### llm-wiki

> "You rarely ever write or edit the wiki manually, it's the domain of the LLM." — Andrej Karpathy, 2026-04-02

You collect sources; the agent compiles them into linked Markdown pages and keeps them current. Three layers (raw sources, wiki, schema file) and three operations (ingest, query, lint), with an index and an append-only log.

**Workflow:** Research to Publication.
**Sources:** [LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), [LLM knowledge bases](https://x.com/karpathy/status/2039805659525644595), [on assistant memory](https://x.com/karpathy/status/2036836816654147718)

→ [SKILL.md](./karpathy-llm-wiki/SKILL.md)

### minimalism

> "Train and inference GPT in 243 lines of pure, dependency-free Python." — Andrej Karpathy, 2026-02-11

Decides, per dependency, whether to keep it, replace it with a small piece of code you own, or remove it. Security-critical code is never hand-written. Also covers designing tools that agents can use.

The 243-line project is microgpt. His earlier minGPT and nanoGPT use PyTorch.

**Workflow:** Idea to Launch.
**Sources:** [litellm attack](https://x.com/karpathy/status/2036487306585268612), [build for agents](https://x.com/karpathy/status/2026360908398862478), [microgpt](https://x.com/karpathy/status/2021694437152157847)

→ [SKILL.md](./karpathy-minimalism/SKILL.md)

### supply-chain-hygiene

> "Supply chain attacks like this are basically the scariest thing imaginable in modern software." — Andrej Karpathy, 2026-03-24

Inspects a package without installing it, pins and hashes what is installed, audits existing projects, and gives a checklist for when a package is compromised. Every finding must come from a command or a fetched page, never from the model's memory.

**Workflow:** Idea to Launch.
**Source:** [litellm attack](https://x.com/karpathy/status/2036487306585268612)

→ [SKILL.md](./karpathy-supply-chain-hygiene/SKILL.md)

### understanding-first

> "you can outsource your thinking but you cannot outsource your understanding" — @yacineMTB, quoted by Karpathy on 2026-04-30

After AI-assisted work, appends a report: key assumptions, what was verified and with what evidence, what was not, and what you should look at yourself.

**Workflows:** Debiased Decision, Monthly Check-up.
**Sources:** [the quote](https://x.com/karpathy/status/2049907410303865030), [jagged abilities](https://x.com/karpathy/status/2049903821095354523), [notes from Claude coding](https://x.com/karpathy/status/2015883857489522876)

→ [SKILL.md](./karpathy-understanding-first/SKILL.md)

### idea-files

> "you just share the idea, then the other person's agent customizes & builds it for your specific needs" — Andrej Karpathy, 2026-04-04

Captures an idea as one Markdown file: summary, core insight, success criteria, smallest useful version, open questions. No code.

**Workflow:** Idea to Launch.
**Sources:** [idea file](https://x.com/karpathy/status/2040470801506541998), [example gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), [Sequoia Ascent highlights](https://x.com/karpathy/status/2049903821095354523)

→ [SKILL.md](./karpathy-idea-files/SKILL.md)

### output-evolution

> "hot tip try ask for HTML" — Andrej Karpathy, 2026-05-11

Chooses the format by reader: self-contained HTML or Marp slides for a person viewing results; Markdown or JSON for agents, terminals and repositories.

**Use for** anything shared or presented. **Not for** a short answer you read once.
**Workflow:** Research to Publication.
**Sources:** [HTML output](https://x.com/karpathy/status/2053872850101285137), [LLM knowledge bases](https://x.com/karpathy/status/2039805659525644595)

→ [SKILL.md](./karpathy-output-evolution/SKILL.md)

### education-first

> "I remain deeply passionate about education and plan to resume my work on it in time." — Andrej Karpathy, 2026-05-19

Turns a project or concept into a teaching version: a single runnable file of under about 300 lines, a walkthrough of why each part exists, exercises, and the common misconception.

**Workflows:** Research to Publication, Monthly Check-up.
**Sources:** [microgpt](https://x.com/karpathy/status/2021694437152157847), [joining Anthropic](https://x.com/karpathy/status/2056753169888334312)

→ [SKILL.md](./karpathy-education-first/SKILL.md)

### system-prompt-learning

> "It feels more like taking notes for yourself" — Andrej Karpathy, 2025-05-11

When an agent repeats a mistake, writes the lesson as `when / do / because` into the instructions it will read next time, tests it, and keeps the instruction file free of duplicates and conflicts.

**Workflow:** Debiased Decision.
**Sources:** [system prompt learning](https://x.com/karpathy/status/1921368644069765486), [take LLMs to school](https://x.com/karpathy/status/1885026028428681698), [environments](https://x.com/karpathy/status/1960803117689397543)

→ [SKILL.md](./karpathy-system-prompt-learning/SKILL.md)

### practice-environments

> "They have to read, and they have to practice." — Andrej Karpathy, 2025-01-30

Builds tasks with automatic scoring and feedback, holds some tasks back for a final test, and writes down what was learned so that it lasts.

**Use for** evaluating an agent or practising a skill. **Not for** open creative work with no checkable answer.
**Workflow:** Monthly Check-up.
**Sources:** [take LLMs to school](https://x.com/karpathy/status/1885026028428681698), [environments](https://x.com/karpathy/status/1960803117689397543)

→ [SKILL.md](./karpathy-practice-environments/SKILL.md)

### meta-reflection

> "I've never felt this much behind as a programmer." — Andrej Karpathy, 2025-12-26

A monthly review built on the tasks you actually did: which skills to keep sharp, which to hand over, which new abilities to build, and a three-item plan for next month.

**Workflow:** Monthly Check-up.
**Sources:** [behind as a programmer](https://x.com/karpathy/status/2004607146781278521), [notes from Claude coding](https://x.com/karpathy/status/2015883857489522876)

→ [SKILL.md](./karpathy-meta-reflection/SKILL.md)

---

## Install

### Claude Code: as a plugin

This repository is its own plugin marketplace. Inside Claude Code 2.1.275 or later:

```
/plugin install karpathy-skills --marketplace D3c3453d/andrej-karpathy-skills
```

On older versions, in two steps:

```
/plugin marketplace add D3c3453d/andrej-karpathy-skills
/plugin install karpathy-skills@andrej-karpathy-skills
```

From a terminal, the same commands start with `claude plugin` in place of `/plugin`.

- All 15 skills are installed together, and Claude Code manages them: `claude plugin update karpathy-skills@andrej-karpathy-skills` fetches a newer release (restart to apply), and `claude plugin uninstall karpathy-skills@andrej-karpathy-skills` removes it.
- Installed this way, each skill's name carries the plugin prefix, for example `/karpathy-skills:karpathy-methodology`.
- Use either the plugin or `install.sh --target claude`, not both, or every skill appears twice.

### Any agent: the install script

```bash
git clone https://github.com/D3c3453d/andrej-karpathy-skills.git
cd andrej-karpathy-skills
bash install.sh
```

By default the script installs to `~/.agents/skills` only. Each agent reads a different directory, so install for one agent at a time:

```bash
bash install.sh --target claude
bash install.sh --target claude --target hermes
bash install.sh --target all          # every directory in the table
bash install.sh --dry-run             # show what would be written, change nothing
```

| Target | Directory |
|---|---|
| `codex` (default) | `~/.agents/skills` |
| `codex-native` | `~/.codex/skills` |
| `claude` | `~/.claude/skills` |
| `openclaw` | `~/.openclaw/skills` |
| `hermes` | `~/.hermes/skills` |
| `clacky` | `~/.clacky/skills` |
| `opencode` | `~/.config/opencode/skills` |
| `opencode-legacy` | `~/.opencode/skills` |

- **Installing replaces** any existing `karpathy-*` folder of the same name in the target directory, including any edits you or an agent made to the installed files.
- `KARPATHY_SKILLS_TARGETS="codex,claude"` sets targets through the environment. It is used only when no `--target` is given.
- If the agent is already running, restart it or reload its skills.

To install a single skill, ask your agent:

```
Install this skill: https://github.com/D3c3453d/andrej-karpathy-skills/tree/main/<skill-name>
```

---

## About

This is the English edition of [LearnPrompt/andrej-karpathy-skills](https://github.com/LearnPrompt/andrej-karpathy-skills), which was inspired by [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills).

The upstream author collected Karpathy's AI-related posts from 2023 to 2026 with the help of Grok, grouped the recurring methods into 14 skills and added the router. The methods are split into separate skills, not one long prompt, because they are different kinds of action: an engineering rhythm, a research loop, a security audit, a personal review. Separate skills can be triggered alone or combined into the four workflows.

What this edition changed:

- Everything is in English.
- Every cited post was fetched and read on 2026-11-15. Quotations are now verbatim, and several were corrected or reattributed. Two lines the original credited to Karpathy were written by other people whom he quoted.
- Details from the posts that the original left out were restored, for example the wiki's schema file and log, and the keep-or-reset rule of the autoresearch loop.
- Each skill follows one layout written for the agent that executes it.
- Commands that did not exist were replaced, and steps that invited a model to invent evidence were rewritten to require real output.

Karpathy announced on 2026-05-19 that he had joined Anthropic. The methods here come from posts before and around that date.

---

<div align="center">

[MIT License](./LICENSE) · free to use, modify and redistribute

Original skill pack by [LearnPrompt](https://github.com/LearnPrompt). The ideas behind the skills come from posts by [@karpathy](https://x.com/karpathy); the procedures are this repository's.

</div>
