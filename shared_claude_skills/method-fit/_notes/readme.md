---
version: 0.1
---

# Notes on writing SKILL.md files

Only `SKILL.MD` is read from the `skill name folder`. everything else is ignored. So `.claude\skills\method-fit\_notes\readme.md`  is **not** read by Claude when the skill runs — Claude only reads `SKILL.md` itself. This file is for humans; it survives decoupled from `SKILL.md` git history.

## What gets loaded, and when

- Before any skill is invoked, only the YAML frontmatter's `description` (and `name`) is loaded into context, for every skill in the project — that's what Claude matches a request against to decide which skill fits.
- Once a skill is invoked (via the Skill tool, or `/skill-name`), the entire file loads, headings and all.

## Headings are not keywords

No heading in the body (`# Role`, `# Output`, `# Procedure`, etc.) is specially parsed. They're just the author's own labels for organizing instructions. Claude follows the words under a heading because they're clear instructions, not because the heading name triggers reserved behavior.

The only actually-parsed keys are the YAML frontmatter fields (`name`, `description`, `license`, etc.).

## What each heading contributes (observed from buyer-fit / method-fit)

- **Role** — sets the reviewing persona/lens (who Claude is pretending to be, what it judges and what it excludes).
- **Procedure** — the steps to follow.
- **Output** — defines the report's actual vocabulary and structure (field names, verdict taxonomy, ranking order). This is the section that most controls what the final report looks like.

Removing the **Output section** doesn't stop Claude from producing a report — it'll improvise generic structure, but won't reliably use the skill's specific vocabulary (e.g. the "Supported/Contradicted/Unsupported/Stale" verdict taxonomy) or guaranteed severity ordering.

## HTML comments don't hide content from Claude

`<!-- ... -->` is an HTML-rendering convention. Claude reads the raw file text, so a commented-out section is still visible and may still be acted on. It does not work as a way to disable a section while keeping it in the file.

To actually disable something: delete it (recoverable via git log), or move it out of `SKILL.md` into a sibling file/folder (like this one) that nothing instructs Claude to read.
