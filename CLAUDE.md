<!-- gitnexus:start -->
# GitNexus — Code Intelligence

This project is indexed by GitNexus as **game-config-data** (46 symbols, 41 relationships, 0 execution flows). Use the GitNexus MCP tools to understand code, assess impact, and navigate safely.

> If any GitNexus tool warns the index is stale, run `npx gitnexus analyze` in terminal first.

## Always Do

- **MUST run impact analysis before editing any symbol.** Before modifying a function, class, or method, run `gitnexus_impact({target: "symbolName", direction: "upstream"})` and report the blast radius (direct callers, affected processes, risk level) to the user.
- **MUST run `gitnexus_detect_changes()` before committing** to verify your changes only affect expected symbols and execution flows.
- **MUST warn the user** if impact analysis returns HIGH or CRITICAL risk before proceeding with edits.
- When exploring unfamiliar code, use `gitnexus_query({query: "concept"})` to find execution flows instead of grepping. It returns process-grouped results ranked by relevance.
- When you need full context on a specific symbol — callers, callees, which execution flows it participates in — use `gitnexus_context({name: "symbolName"})`.

## Never Do

- NEVER edit a function, class, or method without first running `gitnexus_impact` on it.
- NEVER ignore HIGH or CRITICAL risk warnings from impact analysis.
- NEVER rename symbols with find-and-replace — use `gitnexus_rename` which understands the call graph.
- NEVER commit changes without running `gitnexus_detect_changes()` to check affected scope.

## Resources

| Resource | Use for |
|----------|---------|
| `gitnexus://repo/game-config-data/context` | Codebase overview, check index freshness |
| `gitnexus://repo/game-config-data/clusters` | All functional areas |
| `gitnexus://repo/game-config-data/processes` | All execution flows |
| `gitnexus://repo/game-config-data/process/{name}` | Step-by-step execution trace |

## CLI

| Task | Read this skill file |
|------|---------------------|
| Understand architecture / "How does X work?" | `.claude/skills/gitnexus/gitnexus-exploring/SKILL.md` |
| Blast radius / "What breaks if I change X?" | `.claude/skills/gitnexus/gitnexus-impact-analysis/SKILL.md` |
| Trace bugs / "Why is X failing?" | `.claude/skills/gitnexus/gitnexus-debugging/SKILL.md` |
| Rename / extract / split / refactor | `.claude/skills/gitnexus/gitnexus-refactoring/SKILL.md` |
| Tools, resources, schema reference | `.claude/skills/gitnexus/gitnexus-guide/SKILL.md` |
| Index, status, clean, wiki CLI commands | `.claude/skills/gitnexus/gitnexus-cli/SKILL.md` |

<!-- gitnexus:end -->

<!-- NEXLINK_HANDBOOK_SYNC_START -->
## NexLink Engineering Handbook synchronization

Primary System: TBD

### Obsidian handbook access

The NexLink Engineering Handbook is stored in the
"NexLink Knowledge Base" Obsidian vault.

Use the configured Obsidian MCP tools to search and read the relevant handbook
notes before making architectural or behavioral decisions.

After completing meaningful work, update the appropriate Obsidian notes when
the work changes documented architecture, capabilities, public APIs,
externally observable behavior, security assumptions, deployment procedures,
ADRs, current status, known problems, open questions, or roadmap status.

Do not merely recommend documentation updates. Make the updates during the
same task when Obsidian MCP access is available.

If an update is required but Obsidian cannot be accessed, do not claim that
the handbook was updated. Report:

- Handbook update required but blocked: <reason>
- Notes that need updating: <note names or subjects>

When this repository belongs to or affects a documented NexLink System, Capability,
Product, or Project, load the NexLink Engineering Handbook before making decisions.

Load context general-to-specific:

1. Company
2. Primary System
3. Relevant Capabilities
4. Consuming Product, when applicable
5. Repository-specific documentation

Before finishing a task, review whether the work changes:

- Architecture
- Capabilities
- Public APIs
- Externally observable behavior
- Security assumptions
- Deployment, rollback, or operational procedures
- ADRs
- Current Status
- Known Problems
- Open Questions
- Roadmap status

When one of these changes, update the appropriate handbook notes as part of the
same task.

Do not update the handbook for internal refactors, formatting-only changes,
local renames, or implementation details that do not alter documented behavior.

Preserve historical ADR wording unless an ADR is explicitly amended or
superseded. Keep handbook changes incremental and do not reorganize its structure.

Follow any preview-and-approval rules defined by the handbook before editing
existing documentation.

At completion, report one of:

- Handbook updated: <notes changed>
- Handbook reviewed; no update required
<!-- NEXLINK_HANDBOOK_SYNC_END -->
