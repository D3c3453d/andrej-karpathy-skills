---
name: karpathy-minimalism
description: 'Apply Karpathy-style minimalism to dependencies and tool design: decide whether a package is needed at all, replace small ones with code you own, and make tools easy for agents to use. Use this skill when the user wants to reduce dependencies, audit a project for bloat, rewrite something in plain standard-library code, design an agent-friendly CLI, or says "too many dependencies", "minimize deps", "pure python", "yoink this", "no frameworks", "agent-native design".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-agentic-engineering
  - karpathy-supply-chain-hygiene
  - karpathy-vibe-to-agentic
  - karpathy-education-first
---

# Minimalism and Agent-Native Design

Every dependency adds code nobody on the project has read and people the project must trust. When the functionality you need is small and simple, write or copy it instead of adding a package. Design tools so that agents can use them.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- Someone, including you, is about to add a dependency.
- The user wants a project's dependencies reviewed or reduced.
- The user wants a tool or CLI designed for use by agents.

## When not to use

- The dependency is already justified and the question is how to install it safely. Use `karpathy-supply-chain-hygiene`.
- The user wants a from-scratch reimplementation in order to learn. Use `karpathy-education-first`.

## Inputs

- **The dependency or dependencies**: a package name, or the project's manifest file (required for dependency work).
- **For tool design instead:** the tool and the actions it must support.
- **What is needed from each**: the specific functions or behaviour, not the package as a whole. Find this by reading how the code uses it.
- **Language and runtime.**

## Procedure

### Decide, for each dependency

1. **Name the need.** Write down exactly which functionality the project uses.
2. **Security-critical or hard domain?** Cryptography, authentication, TLS, sandboxing, parsers for untrusted input, time zones, numerical methods. If yes, use a maintained library and go to step 5. Never write these yourself.
3. **Small and simple?** If the needed functionality fits in about 200 lines that you can read and test in full, write it yourself and go to step 4. This number is this repository's rule of thumb.
4. **Write or copy it ("yoink").**
   - Take only the functions the project needs.
   - If you copy code, check that its licence allows it, keep the licence notice, and add a comment: `# adapted from [project] [version], [licence], [date]`.
   - Add tests for it.
   - Before removing the package, run both implementations on inputs taken from the project's tests and data, and show the user every difference.
   - Tell the user the project now owns this code, including its bugs.
5. **Otherwise use the package**, but being well known proves nothing about safety: litellm, whose release 1.82.8 was poisoned in March 2026, has 97 million downloads a month. Count what it pulls in, then run `karpathy-supply-chain-hygiene` before installing.

Counting transitive dependencies:

```bash
pipdeptree --packages [package]         # Python, package already installed
npm ls --all                            # Node, full tree of the current project
# Not installed yet: resolve without installing
pip install --dry-run --ignore-installed --only-binary=:all: --report - "[package]==[version]"
npm view [package] dependencies         # declared dependencies only
```

If a tool is missing, ask before installing it, or write UNKNOWN.

### Review an existing project

1. List every direct dependency with what the code uses it for. Find the uses by searching the code, not by guessing from the name.
2. Give each a verdict and a reason: KEEP, REPLACE (write it yourself, per steps 2 to 4) or REMOVE (no import found, and not referenced in configuration, scripts, plugins or CI).
3. Show the user the table. Remove or replace nothing without their approval.
4. Make each approved change separately and run the tests after each one.

### Template for a small replacement

```python
#!/usr/bin/env python3
"""slugify: minimal implementation, standard library only.

Adapted from: [project] [version], [licence], [date]   (omit this line if nothing was copied)
Usage:  python3 slugify.py "Some Title"
Import: from slugify import slugify
"""
import re
import sys


def slugify(text: str) -> str:
    """Lowercase the text and join its alphanumeric runs with hyphens."""
    return "-".join(re.findall(r"[a-z0-9]+", text.lower()))


if __name__ == "__main__":
    if len(sys.argv) != 2:
        sys.exit('usage: python3 slugify.py "text"')
    print(slugify(sys.argv[1]))
```

### Design a tool for agents

For this mode, produce a table of the seven points below, each marked met or not met, with the change you propose. Write no code unless asked.

Three of the questions Karpathy asks of any product or service:

- Are the docs available as Markdown?
- Is there a skill, an instruction file an agent can load, for it?
- Can it be used from a command line or through MCP?

This repository adds:

- Every action is reachable through documented flags, with `--help`; nothing requires an interactive prompt.
- Output is plain text or JSON that another program can read.
- Errors say what failed, what was expected and what to try next.
- Configuration lives in commented text files.

## Output

For tool design: the seven-point table. For dependency work: a table with one row per dependency: what it is used for, number of transitive dependencies (with the command that produced it), verdict, reason. Then the list of changes made, each with its test result.

## Done when

- [ ] Every dependency kept has a stated reason beyond convenience.
- [ ] Nothing security-critical was replaced with hand-written code.
- [ ] Copied code carries its licence and origin.
- [ ] Every count in the table came from a command, not from memory.
- [ ] Tests pass after each removal or replacement.
- [ ] For tool design: each of the seven points is marked met or not met, with a proposed change.

## Workflow

Step 3 of **Idea to Launch**. It reviews the dependencies that the build in `karpathy-agentic-engineering` introduced, then passes the ones that remain to `karpathy-supply-chain-hygiene`. Its rules also apply during the build, whenever a new dependency is proposed.

## Source

- After the litellm attack, Karpathy wrote on 2026-03-24 that he has grown averse to dependencies and prefers to have an LLM copy functionality "when it's simple enough and possible". He gave no line limit. https://x.com/karpathy/status/2036487306585268612
- The agent-design questions are from his post of 2026-02-24, which asks of every product: "can agents access and use them?" https://x.com/karpathy/status/2026360908398862478
- His 243-line GPT (2026-02-11) shows how small a complete implementation can be: https://x.com/karpathy/status/2021694437152157847

The procedure, templates and thresholds here are this repository's elaboration.
