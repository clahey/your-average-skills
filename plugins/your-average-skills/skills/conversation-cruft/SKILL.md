---
name: conversation-cruft
description: Find and remove conversation cruft from docs and code comments — narration of how something changed, meaning that only resolves with conversation context, or rebuttals to questions only raised in discussion — leaving the current, settled state stated once, cleanly. Preserves genuine decision-history/rationale sections (tightens them to the eventual motivation; never deletes them wholesale). Use when asked to "clean up", "tidy up", "remove the history from", "make this read fresh", or similar, for a doc, a code comment, or a diff. Scope can be the whole project, uncommitted changes, or a branch diff.
---

# conversation-cruft

Strip conversation cruft from written artifacts (docs, code comments) so they
read as if authored fresh today by someone with no memory of the conversation
that produced them — while preserving genuine rationale and decision-history
content, which is not cruft.

## The test

For any given line or passage: **would a fresh author, who knew only the
current, settled state and nothing of the conversation that produced it, write
this?** If yes, keep it. If no, it's cruft — fix it.

## What counts as cruft

1. **Narration of how something changed.** "Originally X, but then we realized
   Y, so now Z." "Revisited once we found..." "Considered and rejected..."
   "Looked like A at first but turned out to be B." "This settled on..."
   State the current fact plainly instead of narrating the path to it.
2. **Meaning that only resolves for someone who was in the conversation.**
   References to "the discussion above," "as mentioned," or an implicit
   pointer to a specific prior exchange rather than to the artifact's own
   content.
3. **Answers or rebuttals that exist only because a question was raised in
   conversation.** Defensive asides addressing a concern nobody reading the
   artifact cold would independently have.

   These often take the shape of an **emphatic denial** — "it doesn't know
   X," "learns nothing about Y," "never sees Z," "has no idea whether W" —
   and especially a denial that escalates through several things not known.
   Emphasis clusters where someone was once reassured. The denial is worth
   keeping when the artifact has to establish the boundary and nothing else
   does; it is cruft when the fact is already stated plainly somewhere the
   reader has passed, and the passage adds insistence rather than content.
4. **Repetition of the same resolved fact at every place it's touched on.**
   One clean mention of "we chose X over Y because Z" belongs, in the place
   built to hold it. Restating "(but not Y)" every other time X comes up
   elsewhere in the document is cruft — consolidate to the one place, and
   let other mentions stand on their own or carry a single pointer.

## What is NOT cruft — never strip wholesale

- **Decision-history / rationale / "Decisions & Alternatives" sections** —
  any section whose actual purpose is to record why a choice was made and
  what was rejected. These are load-bearing content, not cruft. Tighten
  them to state the *eventual motivation* cleanly (what was chosen, what was
  rejected, why) — strip the back-and-forth narration of *how* the
  conversation arrived there, but keep the substance. A decision-history
  section should end up shorter and cleaner after this pass, never emptied
  or removed.
- Rationale, named alternatives, and constraints a fresh author would
  independently write. Only the *narration* of arriving at them is cruft —
  the content itself is present intent.
- Code comments explaining non-obvious WHY (a hidden constraint, a subtle
  invariant, a workaround for a specific bug). These stay. Only comments
  that reference the current task, a specific fix, or a conversation ("//
  fixed for issue #123," "// changed this per discussion," "// used by the
  new X flow") are cruft — and per a no-comments-by-default convention,
  these should usually just be deleted rather than rewritten into something
  more generic, unless removing the comment would lose a genuinely
  non-obvious WHY that's worth keeping in a fresh, task-agnostic form.

## Scope of a run

Infer scope from how the skill is invoked; don't demand ceremony beyond
what's needed to disambiguate.

- **Whole project** — no target given, or explicitly "whole project" / "all
  docs": scan docs (READMEs, design docs, HLD/LLD-style docs, etc.) and code
  comments across the tree.
- **Uncommitted changes** — "uncommitted," "working tree," "my changes":
  scope to `git diff` / `git status` output — modified and added lines only.
- **Branch diff** — "this branch," "vs main," a named base: scope to `git
  diff <base>...HEAD` (default base: the repo's usual trunk, e.g. `main` or
  `master`, unless told otherwise).
- If scope is genuinely ambiguous — no argument, no single obvious target,
  more than one plausible reading — ask rather than guessing. This pass
  rewrites text in place; a wrong guess about scope is wasted work at best
  and an unwanted edit at worst.

## Process

1. Determine scope (above).
2. Read the target files or diff — the prose itself, in full, not a keyword
   scan of it. Searching for trigger words finds some rule 1 cruft and misses
   rules 2–4 entirely, because a rebuttal, a stale pointer, and a duplicated
   fact contain no distinguishing token: each reads as ordinary prose and is
   only visible against what the rest of the artifact already says. On a large
   diff the temptation to grep instead is exactly proportional to how much
   there is to miss.
3. For each doc or comment, identify cruft against the rules above. Flag
   it — don't silently leave it — but don't flag well-written rationale or
   decision-history content that legitimately discusses alternatives.
4. Rewrite in place:
   - Consolidate repeated mentions of the same resolved fact into one clean
     statement, placed in whichever section structurally owns it (a
     Decisions section, a Non-Goals entry, etc.); trim other mentions to a
     brief pointer or remove them if they add nothing beyond the repetition.
   - For code comments that are pure cruft, delete them rather than
     rewriting, consistent with a no-comments-by-default default — unless
     the file/project has an established convention of keeping a changelog-
     style comment block, in which case respect that convention.
   - For decision-history sections, tighten to the eventual motivation;
     never delete the section or empty it out.
5. Report a short summary: what was trimmed or consolidated, and what was
   deliberately preserved as legitimate rationale/decision-history — so the
   line drawn is easy to sanity-check.

## Notes

- This is a judgment pass, not mechanical find-and-replace. Apply the
  fresh-author test per instance rather than pattern-matching on trigger
  words like "originally" or "we decided" — those words also show up in
  legitimate rationale that should stay.
- When genuinely unsure whether something is cruft or load-bearing
  rationale, keep it and flag it in the summary rather than silently
  deleting real content.
