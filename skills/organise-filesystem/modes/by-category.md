# Mode: by-category

Sort by subject/category/subcategory rather than by date or media type.

## Strategy

1. **Sample, don't scan-all** — for >500 files, sample ~50 representative files (random + largest + recently-touched) to infer the category schema before classifying everything.
2. **Propose schema first** — return a proposed top-level taxonomy to the user before moving anything. Example output:
   ```
   Proposed categories:
     - finance/        (invoices, receipts, bank statements)  142 files
     - work/clients/   (per-client subfolders)                 89 files
     - personal/id/    (passports, licenses, certs)            12 files
     - personal/medical/                                       28 files
     - reference/                                              60 files
     - _review/        (couldn't place)                         5 files
   ```
3. **Subcategorise only when count justifies it** — see recursion-depth rule in `SKILL.md`.

## Classification signals (in order of trust)

1. **File content** — read text/PDF/doc content for the first ~2KB; use vision on images. Highest trust.
2. **Filename keywords** — `invoice`, `receipt`, `contract`, `passport`, screenshot prefixes.
3. **Sibling context** — if a folder is already named `invoices/`, treat its contents as priors.
4. **Sender / metadata** — email-saved files often retain `From <name>` in metadata.

## Conservatism

- A file matching multiple categories goes to the most specific. If genuinely ambiguous → `_review/`.
- Never invent a category for a single file. Single oddballs → `misc/` or `_review/`.
- Don't auto-create more than ~8 top-level categories. If the data demands more, group them.

## Cross-mode composition

`by-category` typically pairs with `by-date` *inside* each category folder (e.g. `finance/invoices/2024/03/`).
