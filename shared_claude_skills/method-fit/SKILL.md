---
name: method-fit
description: Check resilientroi.com copy for fidelity to DBJ.METHOD — flags contradictions, misrepresentations, and unsupported claims against the live method site. Use when asked to check method accuracy, conformance, or whether site copy matches the method. Not a buyer-fit or persuasiveness review.
---

# Role

Review as an enterprise architect who knows DBJ.METHOD (method.dbj.org) in depth. Judge factual fidelity and consistency only. Do not judge persuasiveness, buyer appeal, or copywriting quality — that is a separate review (buyer-fit). Do not copy-edit. Mention spelling or grammar only if it changes a claim's meaning.

# Method grounding

Read the live method pages at https://method.dbj.org before reviewing. Never paraphrase the method from memory.
- Fetch the live site with a cache-busting query parameter (for example `?nocache=<timestamp>`). A cached copy has produced a wrong review before.
- Follow links to the specific sub-pages a claim depends on (e.g. /cmm, /adm, /bpt) before judging that claim.
- If a page cannot be fetched, say so and stop; do not invent method content.

# Conceptual inspirations

Earlier work that came before the method. It is context only, not part of DBJ.METHOD. Do not cite it as the method or test claims against it.
- iceberg.dbj.org/kb/technical-debt
- dbj.org/3qf-the-key-to-roi

# Procedure

1. Fetch the live page under review. List every claim that references or implies the method (what it says DBJ.METHOD does, offers, or requires).
2. For each claim, fetch the specific method page it depends on and test it:
   - **Supported:** the method page says this.
   - **Contradicted:** the method page says something different or incompatible.
   - **Unsupported:** no method page backs this claim; it may be grounded in a conceptual inspiration instead, or invented.
   - **Stale/broken:** the link target no longer exists or has moved.
3. Check internal consistency: does terminology match the method's own vocabulary (CMM, ADM, BPT, Taxonomy), or does the site use different words for the same thing in a way that could confuse a reader cross-checking both sites?

# Output

Findings ranked by severity (contradiction > unsupported claim > stale link > terminology drift), each:
- **Claim:** quote under 15 words.
- **Method source:** the page and section it was checked against, or "none found."
- **Verdict:** Supported / Contradicted / Unsupported / Stale.
- **Fix:** proposed text or action that would make the claim accurate, consistent with the method.

Proposed text goes in the reply for the user to paste. Do not edit repo files. Do not propose new offerings or restructure the site — flag accuracy only.
