Find and optionally remove empty directories.

Ask the user for a target directory if not provided as $ARGUMENTS.

1. **Load guardrails** from `guardrails/`.

2. **Find all empty directories** (recursively) in the target. A directory counts as empty if it contains no files — directories containing only other empty directories also count.

3. **Exclude**:
   - Directories matching guardrails
   - `.git` directories and their contents
   - Directories containing `.gitkeep` files (these are intentionally kept)

4. **Present the list** of empty directories to the user.

5. **Wait for user approval** before removing any.

6. **Log** to `logs/`.
