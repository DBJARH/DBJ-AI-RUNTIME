---
version: 0.1
---

# Recipes

Situations, and the rules that fix them.

## The file is full of one-off entries

Symptom: hundreds of lines like
`Bash(cp "g:/a/b.md" "g:/c/b.md")`, still prompting constantly.

Each was created by answering "yes, allow this exact command". They never fire
twice. Delete them and keep one rule per command family:

```json
"Bash(cp:*)", "Bash(mv:*)", "Bash(mkdir:*)"
```

Back up before the cull:

```sh
cp ~/.claude/settings.json ~/.claude/settings.json.bak
```

## Prompted for `ls`, `cat`, `git status`

The read-only basics were never allowed. Take the inspection block from
[relaxed.json](profiles/relaxed.json), or minimally:

```json
"Bash(ls:*)", "Bash(cat:*)", "Bash(head:*)", "Bash(tail:*)",
"Bash(grep:*)", "Bash(find:*)", "Bash(wc:*)", "Bash(pwd)",
"Bash(git status:*)", "Bash(git log:*)", "Bash(git diff:*)"
```

## All of git except the dangerous parts

```json
"allow": [ "Bash(git:*)" ],
"deny":  [
  "Bash(git push --force:*)",
  "Bash(git push -f:*)",
  "Bash(git reset --hard:*)",
  "Bash(git clean -fd:*)"
]
```

`deny` is evaluated first, so the narrow rules win over the broad allow.

## Reading a second repository

Two changes, both required:

```json
"additionalDirectories": [ "G:\\REPOS\\OTHER_REPO" ],
"allow": [ "Read(//g/REPOS/OTHER_REPO/**)" ]
```

The directory entry grants access; the rule silences the prompt.

## Never read secrets

```json
"deny": [
  "Read(//**/.env)",
  "Read(//**/.env.*)",
  "Read(//**/id_rsa)",
  "Read(//**/id_ed25519)",
  "Read(//**/.aws/credentials)",
  "Read(//**/.ssh/**)"
]
```

Worth keeping in every profile, including permissive ones. Note this stops
Claude reading them directly; it does not stop a permitted `cat` from
printing one. Add `Bash(cat .env:*)` to `deny` if that matters.

## A team-shared project setup

Commit `<repo>/.claude/settings.json` with the rules everyone needs — the
project's build, test and lint commands:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run build:*)",
      "Bash(npm test:*)",
      "Bash(npm run lint:*)"
    ]
  }
}
```

Personal additions go in `settings.local.json`, which should be gitignored.

## An unfamiliar repository

Install [readonly.json](profiles/readonly.json) as the project's
`.claude/settings.local.json` before opening it. Claude can read and explain,
and cannot modify, install, or reach the network.

## Windows: Bash and PowerShell both prompt

They are separate tools with separate rules. Allowing `Bash(git:*)` does
nothing for `PowerShell(git:*)`. Either list both, or standardise on one
shell.

Also note the Bash tool is Git Bash: `py`, and other Windows-only launchers on
the PowerShell PATH, may not exist there.

## A long build keeps timing out

Not a permissions problem. Run it in the background rather than widening
rules — the timeout is a tool argument, not a permission.

## Auditing what you have

```sh
# rule count
grep -c '"' ~/.claude/settings.json

# the one-off entries — long, path-laden, no :* wildcard
grep -n 'Bash(' ~/.claude/settings.json | grep -v ':\*'
```

The second command lists your prompt-answered accidents. Most can be replaced
by a handful of family rules.

## Vocabulary

**Command family** — all invocations of one executable, written `Bash(cmd:*)`.
The useful granularity for allow rules: you trust the tool, not one argument
list.

**One-off entry** — a rule naming exact arguments, produced by accepting a
permission prompt verbatim. Fires once, never again.
