# Atoka Tech Skills

Reusable agent skills maintained by Atoka Tech.

| Skill | Purpose | Runtime |
| --- | --- | --- |
| [dual-research](skills/dual-research/SKILL.md) | Independent Claude and Codex research, evidence comparison, and synthesis | Claude Code with the Codex plugin |

## Install

After this repository is publicly available on GitHub:

```bash
npx skills add atoka-tech/skills --skill dual-research --agent claude-code
```

Installing the skill does not install its runtime dependencies.

## Dual Research Requirements

- Claude Code as the coordinator.
- The [Codex plugin for Claude Code](https://github.com/openai/codex-plugin-cc)
  installed and configured, with Codex authenticated. Follow the plugin's setup
  instructions before running this skill.
- A shared writable workspace and access to the research sources.
- Optional: a `/grilling` skill for clarification. Normal chat works without it.

This skill's command workflow requires Claude Code, even though the distribution
format can be installed by other agent hosts.

Example request in Claude Code:

```text
Use dual-research to compare our deployment options. Have Claude and Codex
investigate independently, compare the evidence, and produce a recommendation
that identifies remaining uncertainty.
```

Research artifacts are written to `dual-research/{topic}/` in the user's project.
Round summaries are kept short; detailed evidence is stored separately. The
workflow preserves unresolved disagreements rather than forcing consensus.

## Repository Structure

```text
skills/
├── README.md
├── .gitignore
└── skills/
    └── dual-research/
        ├── SKILL.md
        └── references/
            └── prompts.md
```

The outer `skills/` is the repository root. The inner `skills/` is the discovery
container. Add future skills as `skills/{skill-name}/SKILL.md`, matching the
folder name to the frontmatter `name`. The CLI supports this layout; see the
[official discovery documentation](https://github.com/vercel-labs/skills#skill-discovery).

## Check Discovery

From the repository root, list local skills without installing them:

```bash
npx skills add . --list
```

After pushing the files to GitHub, check the published source:

```bash
npx skills add atoka-tech/skills --list
```

The expected skill name is `dual-research`. Listing validates discovery, not the
Claude/Codex workflow. Before release, run a small research task in Claude Code
to check continuation, file creation, and comparison with the installed plugin.

## Publishing on skills.sh

1. Make this repository publicly accessible on GitHub and push the skill files.
2. Check remote discovery with the command above.
3. Share the installation command with users.

According to the [skills.sh FAQ](https://www.skills.sh/docs/faq), skills appear in
the directory and leaderboard through installation telemetry from the skills
CLI. There is no separate skill-upload step described in that workflow. A local
discovery check alone does not establish a listing on skills.sh.

No license has been selected in this repository. Choose an appropriate license
before inviting redistribution or contributions.
