# Sources

Every source below was fetched and read in full on 2026-11-15. Text in quotation marks is verbatim. All posts are by Andrej Karpathy (@karpathy) unless marked otherwise.

The skills in this pack are an unofficial distillation. Each skill's `## Source` section says which parts come from a post and which are this repository's own elaboration.

| Key | Date | Source | What it says | Used by |
|---|---|---|---|---|
| S2 | 2025-01-30 | [post](https://x.com/karpathy/status/1885026028428681698) | "We have to take the LLMs to school." A textbook holds exposition, worked problems and practice problems; these correspond to pretraining, supervised finetuning and reinforcement learning. | practice-environments, system-prompt-learning |
| S3 | 2025-02-02 | [post](https://x.com/karpathy/status/1886192184808149383) | Coins "vibe coding": accept every change, do not read the diffs. "It's not too bad for throwaway weekend projects". | vibe-to-agentic |
| S4 | 2025-04-25 | [post](https://x.com/karpathy/status/1915581920022585597) | His rhythm for code he professionally cares about: relevant context, one incremental change, approaches before code, review, test, commit. | agentic-engineering, vibe-to-agentic |
| S5 | 2025-05-11 | [post](https://x.com/karpathy/status/1921368644069765486) | Proposes "system prompt learning": lessons written down as explicit strategies. "Pretraining is for knowledge." "Finetuning (SL/RL) is for habitual behavior." | system-prompt-learning |
| S6 | 2025-08-27 | [post](https://x.com/karpathy/status/1960803117689397543) | Environments let a model act and see outcomes, for training and evaluation. "I am bullish on environments and agentic interactions but I am bearish on reinforcement learning specifically". | practice-environments, system-prompt-learning |
| S7 | 2025-12-07 | [post](https://x.com/karpathy/status/1997731268969304070) | "Don't think of LLMs as entities but as simulators." Ask what a good group of people would say instead of what the model thinks. | llm-simulator |
| S8 | 2025-12-26 | [post](https://x.com/karpathy/status/2004607146781278521) | "I've never felt this much behind as a programmer." A new layer of agents, prompts, tools and workflows to master. | meta-reflection |
| S9 | 2026-01-26 | [post](https://x.com/karpathy/status/2015883857489522876) | Notes from weeks of coding with Claude: agent failure modes, leverage through success criteria, atrophy of manual coding, a phase shift around December 2025. | agentic-engineering, vibe-to-agentic, understanding-first, meta-reflection |
| S10 | 2026-02-11 | [post](https://x.com/karpathy/status/2021694437152157847), [code](https://gist.github.com/karpathy/8627fe009c40f57531cb18360106ce95) | "Train and inference GPT in 243 lines of pure, dependency-free Python." "Everything else is just for efficiency." The file is `microgpt.py`; the linked copy has been revised since and is now shorter. | education-first, minimalism |
| S11 | 2026-02-24 | [post](https://x.com/karpathy/status/2026360908398862478) | Command-line tools suit agents. Ask of any product: "can agents access and use them?" | minimalism |
| S12 | 2026-02-25 | [post](https://x.com/karpathy/status/2026731645169185220) | In his view coding agents basically started working in December 2025, with caveats. They do best on "tasks that are well-specified and where you can verify/test functionality". | agentic-engineering |
| S13 | 2026-03-07 | [post](https://x.com/karpathy/status/2030371219518931079), [program.md](https://github.com/karpathy/autoresearch/blob/master/program.md) | The autoresearch repository: "the human iterates on the prompt (.md)", "the AI agent iterates on the training code (.py)", in an autonomous loop on a git branch. | autoresearch |
| S14 | 2026-03-24 | [post](https://x.com/karpathy/status/2036487306585268612) | The litellm PyPI release 1.82.8 was poisoned and stole credentials on install. He prefers copying small functionality over adding dependencies "when it's simple enough and possible". | supply-chain-hygiene, minimalism |
| S15 | 2026-03-25 | [post](https://x.com/karpathy/status/2036836816654147718) | Assistant memory features over-apply old context. | llm-wiki |
| S16 | 2026-03-28 | [post](https://x.com/karpathy/status/2037921699824607591) | An LLM improved his argument for hours, then demolished it when asked to argue the opposite. Models are "extremely competent in arguing almost any direction". | llm-simulator |
| S17 | 2026-04-02 | [post](https://x.com/karpathy/status/2039805659525644595) | LLM knowledge bases: raw sources compiled by an LLM into a Markdown wiki. "You rarely ever write or edit the wiki manually, it's the domain of the LLM." | llm-wiki, output-evolution |
| S18 | 2026-04-04 | [post](https://x.com/karpathy/status/2040470801506541998) | The idea file: "you just share the idea, then the other person's agent customizes & builds it for your specific needs". | idea-files |
| S19 | announced 2026-04-04 | [gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) | The LLM Wiki idea file: three layers, three operations, index and log. | llm-wiki, idea-files |
| S20 | 2026-04-30 | [post](https://x.com/karpathy/status/2049903821095354523) | Highlights of his Sequoia Ascent 2026 talk: "install .md skills instead of install .sh scripts", why model ability is jagged, the agent-native economy. | understanding-first, idea-files, vibe-to-agentic |
| S21 | 2026-04-30 | [post](https://x.com/karpathy/status/2049907410303865030) | Reposts a line by @yacineMTB as "the quote I've been citing a lot recently". | understanding-first, methodology |
| S22 | 2026-05-11 | [post](https://x.com/karpathy/status/2053872850101285137) | Ask the model to "structure your response as HTML". Expected progression of output: raw text, Markdown, HTML, and eventually interactive neural video. | output-evolution |
| S23 | 2026-05-19 | [post](https://x.com/karpathy/status/2056753169888334312) | Joins Anthropic. "I remain deeply passionate about education and plan to resume my work on it in time." | education-first |

## Attribution notes

- **"you can outsource your thinking but you cannot outsource your understanding"** was written by @yacineMTB on 2026-02-04. Karpathy quoted it (S21); he did not write it.
- **"vibe coding raised the floor. Agentic engineering raises the ceiling."** is Stephanie Zhan's summary of her conversation with Karpathy at Sequoia Ascent 2026 ([her post](https://x.com/stephzhan/status/2049518659513852109)), which Karpathy quoted in S20.
- Line thresholds, checklists, templates and the four workflows are this repository's own. They are not Karpathy's words.

## Related project

[multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) condenses S9 into four coding guidelines. It inspired this pack and is a separate project.
