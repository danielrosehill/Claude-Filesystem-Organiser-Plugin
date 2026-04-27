# Mode: cleanup

Safe baseline pass. Run this first on any messy tree before applying structural modes.

## Actions

1. **Typo remediation** — fix obvious misspellings without asking (`porject` → `project`, `invoces` → `invoices`, `recieved` → `received`). Don't touch proper nouns, acronyms, or anything that could be intentional.
2. **Naming normalisation** — rename to `snake_case`, lowercase ext. Preserve `Train-Case` if the surrounding tree consistently uses it.
3. **Orphans** — for each loose file at a level, search sibling folders for a semantic match by name/content; move if confident, otherwise leave and flag.
4. **Duplicates** —
   - Exact: SHA-256 match → keep the one with the cleanest name / oldest mtime, delete the rest.
   - Near: same name+size, different mtime → move duplicates to `_review/duplicates/`.
5. **Temp/system files** — delete `.DS_Store`, `Thumbs.db`, `desktop.ini`, `*.tmp`, `*.bak`, `~$*`, browser-download `.crdownload`, `.part`.
6. **Empty folders** — remove only those that were empty before the run OR became empty as a result. Don't touch hidden config dirs (`.git`, `.cache`, etc.).
7. **Archive sweep** — files untouched 6+ months and clearly superseded → `archive/YYYY/`.

## Output

Report counts per action. List any orphans that couldn't be placed.
