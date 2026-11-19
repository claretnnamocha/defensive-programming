# defensive-programming

Portable [Agent Skill](https://agentskills.io/specification) for **all executable logic** — domain rules, state machines, branches, async effects, and trust boundaries — plus integrity patterns for APIs, webhooks, and data.

Structured like multi-reference skills (e.g. [impeccable](https://www.skills.sh/pbakaus/impeccable/impeccable)): `SKILL.md` routes to playbooks under `references/`.

## Install

```bash
npx skills add claretnnamocha/defensive-programming
```

Discover without installing:

```bash
npx skills add claretnnamocha/defensive-programming --list
```

## Cursor

Copy this repository into `.cursor/skills/defensive-programming/` or install via the skills CLI.

## Claude Code / other agents

Install with the same `npx skills add` command when your host supports the Agent Skills format.

## Contents

| Path | Purpose |
| --- | --- |
| `SKILL.md` | Definition, principles, setup, playbook router |
| `references/core-definition.md` | Full definition (McConnell, DbC, fail-fast, contracts) |
| `references/logic-and-invariants.md` | Branches, loops, FSM, pre/post/invariant |
| `references/logic-floor.md` | Minimum bar before any logic edit |
| `references/implement.md` / `review-playbook.md` | Implement and review workflows |
| `references/layers.md` | Prevent → enforce → assist → observe → prove |
| `references/review-checklist.md` | Integrity review rules |
| `references/sources.md` | Public bibliography |
| `examples/annotated-patterns.md` | Generic snippets |

## License

MIT — see [LICENSE](LICENSE).