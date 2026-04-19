Find duplicate files in a target directory.

Ask the user for a target directory if not provided as $ARGUMENTS.

1. **Load guardrails** from `guardrails/` — skip protected paths.

2. **Scan for duplicates** using file name and size comparison:
   - Group files by exact name match across different directories
   - Group files by size where names differ but content may be identical
   - For size-matched files, compare checksums (md5sum) to confirm true duplicates

3. **Present findings** as a table:
   - File name
   - Locations (all paths where the duplicate exists)
   - Size
   - Last modified date for each copy

4. **Recommend which to keep**: Prefer the copy in the most logical location, or the most recently modified version.

5. **Wait for user approval** before deleting anything.

6. **Log results** to `data/duplicates-YYYY-MM-DD-dirname.md`.
