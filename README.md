# design-precedents

An Agent Skill for studying mature systems with structurally similar problems before proposing or substantially changing a design.

Instead of copying surface layouts or name-dropping famous products, it guides agents to extract transferable principles, compare constraints, label verified versus memory-based claims, and settle contested trade-offs with concrete data shapes or small prototypes.

## What it provides

- **Three depth modes**: **Lightweight** (3 lines for local decisions), **Standard** (concise review), and **Deep-dive** (primary-source comparison across contrasting precedent families).
- **Five-step workflow**: structural problem framing, primary-source research with explicit `from memory (unverified)` labeling, Surface/Mechanism/Principle extraction, Borrow/Adapt/Reject/Investigate/New-design classification with novelty and historical-baggage challenges, and concrete type sketches or prototypes when precedents conflict.
- **Cross-domain and industrial perspective**: covers frontend, APIs, backend/distributed systems, and complex realtime/hardware/industrial tooling without forcing SaaS conventions onto domain-heavy systems.

## Repository layout

| Path | Role |
| --- | --- |
| [SKILL.md](SKILL.md) | Main skill entrypoint, depth modes, workflow, output templates, and anti-patterns |
| [references/precedent-guide.md](references/precedent-guide.md) | On-demand reference for design dimensions, cross-domain catalog, comparison matrix, design taste checklist, and worked example |

## Installation

Clone this repository and link (or copy) it into your agent's skills directory:

```bash
git clone git@github.com:NERanger/design-precedents.git ~/repo/design-precedents
```

Example target directories:

```bash
# Codex
ln -s ~/repo/design-precedents ~/.codex/skills/design-precedents

# OpenCode
ln -s ~/repo/design-precedents ~/.config/opencode/skills/design-precedents

# Antigravity / Gemini
ln -s ~/repo/design-precedents ~/.gemini/config/skills/design-precedents

# Devin
ln -s ~/repo/design-precedents ~/.config/devin/skills/design-precedents
```

Replace `ln -s` with `cp -a` if your environment requires standalone copies instead of symlinks.

## Usage

Invoke the skill before choosing an architecture, state model, API/CLI contract, UI workflow, or distributed/industrial system design (for example `/design-precedents` or `$design-precedents`).

Do not use it for routine bug fixes, small local edits, code-only style questions, or post-hoc decoration of an already settled design.
