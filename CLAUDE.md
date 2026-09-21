<!-- gitnexus:start -->
# GitNexus — Code Intelligence

This project is indexed by GitNexus as **game-config-data** (66 symbols, 61 relationships, 0 execution flows).

> Index stale? Run `node .gitnexus/run.cjs analyze --index-only` from the project root — it auto-selects an available runner. No `.gitnexus/run.cjs` yet? Bootstrap with `npx`, `bunx`, or `pnpm dlx` — e.g. `bunx gitnexus@latest analyze` (npm 11 npx crash; #1939).

## Always Do

- **MUST run impact before editing.** Use `impact({target: "symbolName", direction: "upstream"})` or `node .gitnexus/run.cjs impact "symbolName" --direction upstream --repo .`; report callers, processes, and risk. Never substitute grep for graph analysis.
- **MUST analyze graph changes before committing.** Use `detect_changes({scope: "all"})` (MCP) or `node .gitnexus/run.cjs detect-changes --scope all --repo .` (CLI fallback). `partial: true` or `truncated: true` is not a clean check — a zero means unseen, not unaffected; re-run it. For regression review: `detect_changes({scope: "compare", base_ref: "main"})` or `node .gitnexus/run.cjs detect-changes --scope compare --base-ref "main" --repo .`.
- MUST warn on HIGH/CRITICAL `risk` pre-edit; never use `riskSharedAxes` to waive a HIGH/CRITICAL `risk` warning. Compare File/symbol: MCP File omits axes; Graph-RAG expands File.
- **MUST treat `risk: UNKNOWN` as unresolved, not as low.** An empty caller set is not evidence the symbol is unused — it can also mean the callers are not resolvable by the index (plain-object property access, dynamic dispatch, cross-language calls). `impact` pairs `UNKNOWN` with a `riskNote` saying so. Confirm with a text search before treating the symbol as safe to change or delete; do not proceed on the strength of a zero.
- **MUST use `query({search_query: "concept"})` for concepts/flows, `context({name: "symbolName"})` for a named symbol, or `impact` for blast radius, on read-only callers, dependencies, imports, or execution flow.** Graph first; text search only for empty/`UNKNOWN`/literals.
- For security review, `explain({target: "fileOrSymbol"})` lists taint findings (source→sink flows; needs `analyze --pdg`).

## Never Do

- NEVER edit a function, class, or method before MCP/CLI impact analysis.
- NEVER ignore HIGH or CRITICAL risk warnings from impact analysis, and never read `UNKNOWN` as an all-clear — it means the walk could not answer, which is the one verdict that requires confirming by other means.
- NEVER rename symbols with find-and-replace — use `rename` which understands the call graph.
- NEVER commit before MCP/CLI graph change analysis.

## Resources

| Resource | Use for |
| --- | --- |
| `gitnexus://repo/game-config-data/context` | Codebase overview, check index freshness |
| `gitnexus://repo/game-config-data/clusters` | All functional areas |
| `gitnexus://repo/game-config-data/processes` | All execution flows |
| `gitnexus://repo/game-config-data/process/{name}` | Step-by-step execution trace |

## CLI

| Task | Read this skill file |
| --- | --- |
| Understand architecture / "How does X work?" | `.claude/skills/gitnexus-exploring/SKILL.md` |
| Blast radius / "What breaks if I change X?" | `.claude/skills/gitnexus-impact-analysis/SKILL.md` |
| Trace bugs / "Why is X failing?" | `.claude/skills/gitnexus-debugging/SKILL.md` |
| Rename / extract / split / refactor | `.claude/skills/gitnexus-refactoring/SKILL.md` |
| Tools, resources, schema reference | `.claude/skills/gitnexus-guide/SKILL.md` |
| Index, status, clean, wiki CLI commands | `.claude/skills/gitnexus-cli/SKILL.md` |

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
