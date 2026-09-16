---
version: 0.1
---

# user_scope

Tracked copy of DBJ's **user-level** Claude Code files, so they are not lost
when a machine is rebuilt.

User-level means: applies to DBJ everywhere — every folder, every repo, every
machine. Project-specific things do not belong here.

## Files

| File | What it is |
|---|---|
| `CLAUDE.md` | Instructions loaded into every Claude Code session, in full, every time. Keep it short. |

## Where it goes locally

This folder is the source of truth. Copy the file to the machine's user
Claude directory:

| OS | Local path |
|---|---|
| Windows | `%USERPROFILE%\.claude\CLAUDE.md` — e.g. `C:\Users\Korisnik\.claude\CLAUDE.md` |
| Linux / macOS | `~/.claude/CLAUDE.md` |

The `.claude` directory sits in the user's home folder, not in any repo.

## Keeping the two in sync

Copying is manual and one file can drift from the other. After editing either
copy, copy it over the other one and commit.

Windows:

```powershell
Copy-Item .claude\user_scope\CLAUDE.md $env:USERPROFILE\.claude\CLAUDE.md
```

Linux / macOS:

```sh
cp .claude/user_scope/CLAUDE.md ~/.claude/CLAUDE.md
```

## Not to be confused with

`.claude/dbj_claude_permissions/` — permission profiles for `settings.json`.
Different kind of file, different purpose.

---

(c) 2026 by dbj@dbj.org | MIT license
