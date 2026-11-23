---
ver: 0.1
description: "This is  accurate as a community-convention description, not something Claude Code might load automatically. context.md has no native support in Claude Code — It is only read if CLAUDE.md explicitly tells to ('Read context.md before ')"
---

In the context of Anthropic's **Claude** (particularly **Claude Code** and agentic coding workflows), there are two main ways a `context.md` file is used:

1. **A community convention for modularizing `CLAUDE.md`:**
Officially, Claude Code automatically reads a file named **`CLAUDE.md`** (not `context.md`) at the start of every session to load project rules, build commands, and architecture notes. However, because dumping everything into one massive `CLAUDE.md` wastes context window tokens, developers frequently create a separate **`context.md`** (or a `.claude/context.md` / `docs/context.md` file) to hold deeper background—such as domain logic, database schemas, or current sprint goals—and reference it inside `CLAUDE.md` (e.g., *"Read `context.md` before modifying database queries"*).
2. **A state-handoff file across sessions (Memory Persistence):**
Because LLM sessions are stateless once closed or compacted, developers often instruct Claude at the end of a session to summarize what was accomplished, what failed, and what remains into a `context.md` file. The next session starts by reading `context.md` so the agent can pick up right where it left off without re-exploring the entire repository.

### `CLAUDE.md` vs. `context.md`

| Feature | `CLAUDE.md` | `context.md` |
| --- | --- | --- |
| **Native Support** | **Built-in.** Automatically loaded into system context by Claude Code on startup. | **Manual / Convention.** Read only when explicitly referenced or requested. |
| **Primary Purpose** | Hard rules, coding standards, test/build commands, and repo structure. | Deep domain background, active task state, or session-to-session handoff notes. |
| **Update Frequency** | Low (changes only when project conventions or tooling change). | High (often updated per feature, sprint, or coding session). |

### Typical Structure of a `context.md`

When used alongside `CLAUDE.md` or Claude Projects, a `context.md` usually tracks dynamic project state:

* **System Overview:** Brief summary of what the system does and key architectural boundaries.
* **Current State / Active Work:** What is currently in progress, known bugs, or recent refactors.
* **Key Files & Entry Points:** Pointers to the most relevant modules so the agent doesn't burn tokens searching.
* **Decisions & Constraints:** *Why* certain approaches were chosen (and which dead ends to avoid).