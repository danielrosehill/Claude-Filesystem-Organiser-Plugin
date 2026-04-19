Scan the user's filesystem to identify clutter hotspots.

Ask the user for a target directory if not provided as $ARGUMENTS.

1. **Load guardrails**: Read `guardrails/` to know which paths and patterns to skip.

2. **Scan the target directory** (default depth: 3 levels):
   - Count total files and folders at each level
   - Identify loose files not inside a subdirectory
   - Identify potential nesting opportunities (files sharing common prefix, type, or date)
   - Identify potential duplicate files (same name, similar size)
   - Calculate a clutter score: `loose_files / total_items` ratio
   - Note folders that could be nested inside another folder

3. **Rank hotspots**: Sort directories by clutter score, highest first.

4. **Save results**: Write a scan report to `data/` as `scan-YYYY-MM-DD-dirname.md` including:
   - Target directory and date
   - Summary statistics (total files, folders, loose files, duplicates)
   - Clutter score
   - Top hotspots ranked by severity
   - Specific recommendations

5. **Present results** to the user and suggest running `/cleanup` on the worst hotspots.
