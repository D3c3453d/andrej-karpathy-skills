---
name: karpathy-supply-chain-hygiene
description: 'Apply supply-chain security hygiene to software dependencies: inspect a package before installing it, pin and hash what is installed, audit an existing project, and respond when a package is compromised. Use this skill when the user wants to audit project dependencies, is concerned about supply-chain attacks, needs to review a requirements.txt or package.json, or says "dependency audit", "supply chain risk", "check packages", "secure dependencies", "pin versions", "check pypi".'
disable-model-invocation: false
user-invocable: true
related_skills:
  - karpathy-minimalism
  - karpathy-agentic-engineering
  - karpathy-vibe-to-agentic
  - karpathy-understanding-first
---

# Supply-Chain Hygiene

Installing a package runs other people's code with your credentials, and the risk sits anywhere in the dependency tree, not only in the packages you chose. Inspect before installing, pin what you install, and know what to do when a package turns out to be compromised.

**How to use this skill:** You, the agent, carry out the steps below yourself. Text in `[BRACKETS]` is a placeholder: fill it from the conversation or the project, and ask the user for any required value you cannot find. Do not hand a template back to the user unless they ask for a prompt to use elsewhere. Where a step names another `karpathy-*` skill, load it by name or read `../[skill-name]/SKILL.md`; if it is not installed, say so and ask, and do not improvise its procedure.

## When to use

- A dependency is about to be added or upgraded (section A, then B).
- The user wants a manifest or lock file audited (section C).
- The user wants installs hardened: pinning, hashes, isolation (section B).
- A package the project uses has been reported compromised (section D).

## When not to use

- The question is whether the dependency is needed at all. Use `karpathy-minimalism` first.
- The concern is a vulnerability in the project's own code.

## Inputs

- **What to audit**: a package name and version, or the project's manifest and lock files (required).
- **Package manager** in use.
- **Whether you can run commands and reach the network.** If not, every finding is UNKNOWN and you say how the user can check it.

## Procedure

**Rule of evidence, for every section.** Each fact in your report comes from a command you ran or a page you fetched in this session, and you name it. Never state download counts, maintainers, advisories or what an install script does from memory. What you could not check is written as UNKNOWN. If a tool named below is not installed, ask before installing it, or mark the finding UNKNOWN.

### A. Before installing a package

1. **Choose an exact version.** Unless you are fixing a vulnerability, prefer a release that is at least a few days old: the poisoned litellm release was pulled in under an hour. (This cooldown is this repository's advice.)
2. **Inspect without installing.** Installing can run the package's code. Work in `[scratch dir]`, a new empty directory outside the project.

   ```bash
   # Python: fetch the wheel only, list it, unpack it to read
   pip download --no-deps --only-binary=:all: -d [scratch dir] "[package]==[version]"
   python3 -m zipfile -l [scratch dir]/[file].whl
   python3 -m zipfile -e [scratch dir]/[file].whl [scratch dir]/unpacked
   # Node: read lifecycle scripts and dependencies from the registry
   npm view "[package]@[version]" scripts dependencies
   ```

   Look for `.pth` files (Python runs them at every interpreter start; this is how litellm 1.82.8 delivered its payload), install or postinstall scripts, code that reads credentials or environment variables and opens network connections, and encoded or obfuscated blobs.

   If a Python package ships no wheel, never run `pip download`, `pip install` or `pip-compile` on it: they run its build code. Take the archive URL from `https://pypi.org/pypi/[package]/[version]/json` (field `urls[].url`), fetch it with `curl -sLO`, unpack it with `tar -xzf` into the scratch directory, and read `setup.py`, `pyproject.toml` and any `.pth` file.
3. **Check known vulnerabilities** for this exact version in the OSV database (`"ecosystem"` is `PyPI` or `npm`):

   ```bash
   curl -s -d '{"package":{"name":"[package]","ecosystem":"PyPI"},"version":"[version]"}' https://api.osv.dev/v1/query
   ```

4. **List what it pulls in**, without installing:

   ```bash
   pip install --dry-run --ignore-installed --only-binary=:all: --report - "[package]==[version]"
   # Node, in the scratch directory:
   npm install --package-lock-only --ignore-scripts "[package]@[version]" && npm ls --all --package-lock-only
   ```

5. **Check maintenance** on the registry and repository pages you fetch: date of the last release, whether issues get answers.
6. **Give a verdict** with the evidence for each point:
   - AVOID: an unpatched vulnerability that affects the intended use, install-time code you cannot explain, or signs of compromise. Name an alternative you checked in this session, or say that none was found.
   - USE WITH CONDITIONS: acceptable only with stated conditions, such as a pinned version, hashes or a sandbox.
   - USE: nothing above applies.

### B. Installing safely

- Pin exact versions and keep a lock file: `package-lock.json`, `uv.lock`, `poetry.lock`. A `requirements.txt` counts as a lock file only when every package, transitive ones included, is pinned with `==` and hashes. Generate one with `pip-compile --generate-hashes` (pip-tools) and install with `pip install --require-hashes -r requirements.txt`.
- In CI use `npm ci`, which installs exactly what the lock file says.
- Where the project works without lifecycle scripts, install with `npm install --ignore-scripts`.
- Try unfamiliar packages in a container or VM that holds no long-lived credentials.

Write a Dockerfile only if the user asks for one. First make sure `.dockerignore` excludes `.env`, `.git` and key files, because the image copies the project directory.

```dockerfile
# Pin the base image by digest; a tag such as 3.12-slim can change.
# `docker pull python:3.12-slim` prints the current digest.
FROM python:3.12-slim@sha256:[DIGEST]

RUN useradd -m -u 1000 appuser
WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir --require-hashes -r requirements.txt

COPY --chown=appuser:appuser . .
USER appuser
CMD ["python", "app.py"]
```

### C. Auditing an existing project

```bash
pip-audit -r requirements.txt    # known vulnerabilities for a requirements file
pipdeptree                       # dependency tree of the current Python environment
npm audit                        # known vulnerabilities, Node
npm ls --all                     # Node dependency tree
```

- Prefer `pip-audit -r [file]`. A bare `pip-audit` audits the Python environment it runs in, which may not be the project's.
- `pip-audit -r` resolves dependencies, which can run package build code. On a fully pinned file add `--no-deps`.
- `npm audit fix` rewrites the lock file. Run it only with the user's approval and show them the diff.
- Report what you find. Do not pin, upgrade or rewrite anything unless the user asks.

Report one row per direct dependency: pinned exactly, hashes present, number of transitive dependencies, advisories found, install scripts, verdict.

### D. When a package you use is reported compromised

1. Read the advisory itself. Note the affected versions and the time window.
2. **Do not start the affected environment's Python** (`python`, `pip`, `pipdeptree`, `pip-audit`): a `.pth` payload runs on every start. Search files instead:
   - the lock files and their history: `git log -p -- [lock file]`, compared against the advisory's time window, not only the current state;
   - installed copies: `find [environment] -iname '[package]-*.dist-info' -o -name '*.pth'`;
   - CI caches, container images and developer machines;
   - for Node, `npm ls [package]`, which runs no package code.
3. Treat every machine or CI runner that installed the bad version as compromised. Every credential reachable from it must be rotated. List them by name and location only; never print a value. In the litellm case these were SSH keys, cloud credentials, Kubernetes configs, API keys in environment variables, git credentials, CI secrets, database passwords, SSL private keys and crypto wallets; shell history can hold more. Rotation must happen from a machine that never had the bad version.
4. Check whether stolen credentials were already used: account access logs, and unexpected releases of any package the user publishes. Stolen credentials are how such attacks spread to further packages.
5. With the user's go-ahead, pin a known-good version or remove the package, and rebuild images from a clean base.
6. Never state that a machine is clean. Removing the package does not rule out persistence; whether to reimage is the user's decision. Say plainly what you could not check.

Rotating credentials and taking services offline are the user's actions. Prepare the list; do not do these yourself unless asked.

## Output

For an audit: the verdict, then the evidence table, then the actions in priority order. Each row names the command or page it came from. For an incident: where the bad version was found, the credentials to rotate (names and locations only), and what remains unchecked.

## Done when

- [ ] Every finding names its evidence, or is marked UNKNOWN.
- [ ] No package was installed in order to inspect it.
- [ ] Install scripts and `.pth` files were looked for, and any found were read.
- [ ] Transitive dependencies were counted from a command's output.
- [ ] Sections A and B: every dependency is pinned to an exact version and a lock file exists. Committing it is left to the user unless they asked for commits.
- [ ] Section C: unpinned dependencies are reported, not changed.
- [ ] Section D: no secret value was printed, and no machine was declared clean.

## Workflow

Step 4 of **Idea to Launch**. It audits and pins the dependencies that `karpathy-minimalism` decided to keep, then hands the project to `karpathy-vibe-to-agentic`. Its rules also apply during the build, whenever a new dependency is proposed.

## Source

Karpathy's post of 2026-03-24 on the litellm attack: https://x.com/karpathy/status/2036487306585268612

- The PyPI release litellm 1.82.8 itself was poisoned. Installing it was enough to send out SSH keys, cloud credentials, API keys and more.
- Every project depending on litellm was exposed through it; his example is dspy.
- By his account the release was up for less than about an hour, and it was noticed only because a bug in the payload crashed a machine.
- He notes that stolen credentials can then be used to take over more accounts and compromise more packages.
- The post he quotes names the mechanism: a file `litellm_init.pth`.
- His conclusion: "Supply chain attacks like this are basically the scariest thing imaginable in modern software."

The post describes the attack and his aversion to dependencies. Sections A to D are this repository's.

The procedure, templates and thresholds here are this repository's elaboration.
