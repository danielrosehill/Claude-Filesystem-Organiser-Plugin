---
name: super-organise
description: Use for a comprehensive, single-shot reorganisation of a chaotic filesystem tree where the user wants the agent to apply the full deterministic ruleset in one pass — naming, typo fixes, dedupe, domain-specific patterns (image galleries, video collections, mixed media, code repos, document collections, download folders), archive hierarchies, and orphan handling — rather than running individual modes from `organise-filesystem`. Triggers on phrases like "super organise", "fully reorganise this folder", "do the works on this directory", "deep clean and restructure", "organise everything autonomously".
---

# Super Organise

A maximalist, deterministic filesystem reorganisation pass. Use when the user wants one comprehensive operation that applies every applicable rule, rather than the composable target+mode flow in `organise-filesystem`.

The full reference prompt lives in `prompt-reference.md` — read it before acting. Earlier iterations are in `archive/` (kept for provenance; do not load by default).

## When to use this vs `organise-filesystem`

- **`organise-filesystem`** — modular. User picks a target (local / gdrive / android) and one or more modes (cleanup, by-date, by-media-type, etc). Best when the user has a specific intent.
- **`super-organise`** — comprehensive single pass. Best for chaotic dumps where the user wants the agent to detect the dominant pattern (image gallery, mixed media, code repo, downloads, document collection) and apply the full ruleset autonomously.

If the user is vague, prefer `organise-filesystem` with `cleanup` mode first. Only escalate to `super-organise` when scope is clearly "do everything".

## Core procedure

1. **Analyse** — scan tree depth, file types, naming patterns, orphans, duplicates. Identify the dominant domain pattern (see Pattern A–F in `prompt-reference.md`).
2. **Plan** — pick the matching pattern's ruleset; map current → target structure; list deletions explicitly.
3. **Confirm** — surface the plan, scope (file count, size, deletion list). Skip confirmation only if the user has explicitly delegated full autonomy.
4. **Execute** — create folders, fix typos, rename to `snake_case`, move files, dedupe, resolve conflicts.
5. **Verify** — re-walk the tree, validate consistency, report counts and a one-line verdict.

## Domain patterns (summary)

Full rules in `prompt-reference.md`. Quick map:

- **Pattern A — Image galleries**: vision-based renaming, resolution split (thumbs vs full), semantic groupings.
- **Pattern B — Video collections**: resolution hierarchy (4k/1080p/720p/sd), aspect-ratio split, content classification.
- **Pattern C — Mixed media**: split by media type first (`images/`, `videos/`, `audio/`, `docs/`, `archives/`), then technical attributes inside each.
- **Pattern D — Code repos**: separate code from non-code; standard `src/`, `assets/`, `docs/`, `tests/` layout.
- **Pattern E — Document collections**: type + status (`active/`, `drafts/`, `archive/`); year/month for time-sensitive docs.
- **Pattern F — Download folders**: file-type grouping, installer versioning, archive consolidation.

## Universal rules (inherit from `organise-filesystem`)

The naming, date, recursion-depth, safety, and anti-over-editing rules in `organise-filesystem/SKILL.md` apply here too — don't restate them. Read that skill's "Universal principles" section before executing.

## Decision boundary

Autonomous (no consultation):
- Typo fixes, snake_case renames, moves to clearly-correct folders, structural creation, removing temp/system files (`.DS_Store`, `Thumbs.db`, `*.tmp`, `*.bak`, empty folders).

Ask first:
- Deletions (other than the always-safe list).
- Restructures touching >30% of the tree.
- Ambiguous file purposes.
- Multiple equally-valid strategies (let the user pick).

When unsure, drop the item in `_review/` (or `bin/` per the reference prompt) at operation root rather than deleting.

## Reporting

End with: counts moved/renamed/deleted, space reclaimed, `_review/` items needing input, dominant pattern detected, one-line verdict.
