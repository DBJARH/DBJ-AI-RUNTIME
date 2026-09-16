---
version: 0.1
---

# Permission rule syntax

## Shape

```
ToolName(argument-pattern)
```

The bare tool name with no parentheses matches every use of that tool:

```json
"Edit"          // all edits
"Bash(ls:*)"    // ls with any arguments
"Bash(pwd)"     // pwd exactly, no arguments
```

## The three lists

| List | Effect |
|---|---|
| `deny` | Refused. Checked first, beats everything. |
| `ask` | Prompts, even when an `allow` rule would cover it. |
| `allow` | Proceeds silently. |

Anything matching no rule falls through to the session's permission mode —
normally a prompt.

## Bash patterns

The `:*` suffix means "this command with any arguments":

```json
"Bash(git:*)"          // any git command
"Bash(git push:*)"     // git push with any arguments
"Bash(git push)"       // bare git push, nothing after it
```

Prefix matching goes as deep as you write it, so `Bash(git push --force:*)`
in `deny` carves a hole in an allowed `Bash(git:*)`.

### What Bash matching cannot do

Matching is on the command string, not on what the shell will actually run.
A rule for `Bash(ls:*)` does not sanitise:

```sh
ls; rm -rf /some/path        # two commands, one string
ls $(rm -rf /some/path)      # substitution
```

Claude is instructed not to smuggle commands this way, and compound commands
are generally matched against every segment — but do not treat the allow list
as a sandbox. It reduces prompting for work you have already decided to
permit. It is not a security boundary against an adversary. For real
isolation, use a container or a VM.

## File path patterns

Paths are gitignore-style globs. On Windows, forward slashes and a leading
`//` for the drive:

```json
"Read(//g/REPOS/**)"          // everything under G:\REPOS
"Read(//**/.env)"             // any .env, at any depth
"Edit(src/**)"                // relative to the working directory
```

`**` crosses directory boundaries; `*` does not.

## MCP tools

No argument pattern — the whole tool is allowed or not:

```json
"mcp__claude_ai_Gmail__gmail_read_message"
"mcp__claude_ai_Google_Drive__search_files"
```

Some clients accept a whole-server wildcard (`mcp__server__*`); support
varies, so listing tools individually is the portable form.

## additionalDirectories

Not a rule list. It widens which directories Claude may touch at all,
outside the current working directory:

```json
"additionalDirectories": [ "G:\\REPOS", "C:\\Users\\Korisnik\\.claude" ]
```

Note the doubled backslashes — JSON string escaping. A `Read()` rule for a
path outside these directories will not work; both are required.

## defaultMode

Sets the starting permission mode for the session:

| Mode | Behaviour |
|---|---|
| `default` | Prompt on first use of each tool. |
| `plan` | Analysis only; no changes until you approve a plan. |
| `acceptEdits` | File edits proceed silently; other tools still prompt. |
| `bypassPermissions` | No prompts at all. |

`bypassPermissions` disables every guardrail including your own `deny` list.
Reasonable inside a throwaway container, unwise on a machine holding work you
care about.

## Common mistakes

**Single backslashes in Windows paths.** `"G:\REPOS"` is invalid JSON escaping.
Write `"G:\\REPOS"`.

**Expecting a live reload.** Settings are read at startup. Restart Claude Code
after editing them.

**Allowing the exact invocation.** The prompt's remembered rule is that one
command with those arguments. Widen it to `Bash(cmd:*)` by hand, or the file
grows without reducing prompts.

**A Read rule without the directory.** Both `additionalDirectories` and the
rule are needed for paths outside the working directory.

## Vocabulary

**Prefix match** — `Bash(git push:*)` matches any string starting
`git push`. Longer prefixes are more specific and can be denied while a
shorter prefix stays allowed.

**Permission mode** — session-wide default applied when no rule matches.
Toggled during a session; `defaultMode` sets the starting value.

## Sources

- Settings reference — https://docs.claude.com/en/docs/claude-code/settings
- IAM and permissions — https://docs.claude.com/en/docs/claude-code/iam
