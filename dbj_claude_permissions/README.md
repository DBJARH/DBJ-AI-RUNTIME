---
version: 0.1
---

# Claude Permissions Almanah

Reusable Claude Code settings: permission profiles and complete `settings.json`
files, with instructions for applying them on any machine.

Written after cleaning up a `settings.json` that had grown to 240 lines of
single-use command entries — every one of them a permission prompt answered
with "yes, allow this exact command", none of them reusable.

## The content

- [profiles/](profiles/) — drop-in settings files, split by OS:
  `profiles/linux/` and `profiles/windows/`. **Pick your OS folder first**,
  then one profile from it:
  - `relaxed.json` — broad allows, destructive operations denied. Daily driver.
  - `strict.json` — read and inspect freely, prompt before any write or
    network call.
  - `readonly.json` — look, never touch. For unfamiliar or untrusted
    repositories.

  The two OS sets differ only where they must: absolute paths and
  `additionalDirectories`, and the toolchain allows (`PowerShell(...)` and
  `mingw32-make` on Windows; `sudo` denied on Linux). The docs below are
  shared by both.
- [syntax.md](syntax.md) — how permission rules are written and matched.
- [recipes.md](recipes.md) — common situations and the rule that fixes them.

## Quick start

Settings live in three places, in increasing precedence:

| File | Scope | Version control |
|---|---|---|
| `~/.claude/settings.json` | every project | no |
| `<repo>/.claude/settings.json` | one project, shared | yes — commit it |
| `<repo>/.claude/settings.local.json` | one project, just you | no — gitignore it |

To apply a profile globally:

```sh
# back up whatever is there now
cp ~/.claude/settings.json ~/.claude/settings.json.bak

# install the profile — Linux
cp profiles/linux/relaxed.json ~/.claude/settings.json

# install the profile — Windows
cp profiles/windows/relaxed.json ~/.claude/settings.json
```

**Restart Claude Code afterwards.** Settings are read at startup; an editing
session will not notice the change.

The profiles carry only `permissions`. If your existing file has other keys
(`model`, `tui`, `voiceEnabled`, …), merge rather than overwrite — copy the
`permissions` block across and leave the rest alone.

## Adapting a profile

The OS split covers the OS. One thing is still specific to a single
machine and must be edited:

**`additionalDirectories`** — paths outside the working directory that Claude
may reach into. The Windows set assumes `G:\REPOS` and a `Korisnik` home
directory; the Linux set assumes `/home/dbjdbj`. On another machine, change
these to that machine's home and repo root.

Toolchain allows (`hugo`, `gcc`, `node`, …) are listed because they are used
here. Drop what you do not use; an allow for a tool you never run is harmless
but noise.

Everything else is portable.

## Why patterns, not commands

When Claude asks permission, the prompt offers to remember that *exact*
invocation. Accepting builds a file full of entries like:

```json
"Bash(cp \"g:/DBJARH/DBJ_METHOD_STG/docs/ai.md\" \"G:/DBJARH/METHOD_DBJ_ORG/docs/ai.md\")"
```

That rule fires once, for those two paths, and never again. A hundred of them
still prompt on the hundred-and-first copy. `Bash(cp:*)` covers all of it.

The cost of the broad rule is real: it also covers copies you did not
foresee. That is the trade — decide once per command family, at a level where
the decision means something, and use `deny` for the specific things that must
never happen.

## Deny wins

`deny` is checked before `allow`, so a broad allow plus a narrow deny is a
safe combination:

```json
"allow": [ "Bash(git:*)" ],
"deny":  [ "Bash(git push --force:*)" ]
```

All of git is permitted except force-push. Build the rest of the list this
way — wide allows, sharp denies.

## Vocabulary

**Permission rule** — a string like `Bash(ls:*)` matching tool calls Claude
wants to make. Three lists: `allow` (proceed silently), `deny` (refuse
outright), `ask` (prompt even if allowed elsewhere).

**Tool** — a capability Claude invokes: `Bash`, `Read`, `Edit`, `Write`,
`WebFetch`, `PowerShell`, MCP server tools. The name before the parenthesis.

**MCP** — Model Context Protocol. External services (Gmail, Drive, a browser)
exposed to Claude as tools. Their rules are named
`mcp__<server>__<tool>` and take no argument pattern.

**Settings precedence** — local overrides project overrides user. Within one
file, `deny` beats `ask` beats `allow`.

**`additionalDirectories`** — directories outside the current working
directory that Claude may access at all. Separate from, and prior to, the
allow rules: a `Read` rule for a path here does nothing unless the directory
is also listed.

## Sources

- Claude Code settings reference —
  https://docs.claude.com/en/docs/claude-code/settings
- IAM and permissions —
  https://docs.claude.com/en/docs/claude-code/iam
