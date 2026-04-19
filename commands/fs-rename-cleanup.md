Clean up messy filenames in a directory.

Ask the user for a target directory if not provided as $ARGUMENTS.

1. **Load guardrails** from `guardrails/`.

2. **Identify messy filenames**:
   - `Copy of`, `Copy (2) of`, etc.
   - Trailing numbers like `file (1).txt`, `file (2).txt`
   - `Untitled`, `New Document`, `Screenshot from...`
   - Mixed case inconsistencies in the same directory
   - Spaces that should be hyphens or underscores (based on surrounding files' convention)
   - Excessively long filenames

3. **Propose renames** as a table: current name → proposed name.

4. **Wait for user approval**. The user can accept all, reject all, or pick individual renames.

5. **Execute and log** to `logs/`.
