Run a cleanup operation on a target directory.

Ask the user for a target directory if not provided as $ARGUMENTS.

1. **Load guardrails**: Read `guardrails/` for protected paths and patterns.

2. **Load prior scan data**: Check `data/` for an existing scan of this directory. If none exists, run a quick scan first.

3. **Analyse the directory** per the core rules in CLAUDE.md:
   - Loose files that should be nested
   - Folders that should be nested inside other folders
   - Obvious duplicates
   - Files with broken names (`Copy of`, `(1)`, `Untitled`, etc.)
   - Cross-reference against guardrails — skip anything protected

4. **Propose a plan**: Present a numbered list of proposed changes:
   - Files to move (source → destination)
   - Folders to create
   - Folders to nest
   - Duplicates to remove
   - Items skipped due to guardrails

5. **Wait for user approval** before executing anything.

6. **Execute in batches** after approval:
   - Create new directories first
   - Move files and folders
   - Remove duplicates last
   - Report after each batch

7. **Log the operation** to `logs/cleanup-YYYY-MM-DD-dirname.md` including:
   - Date, time, target directory
   - All actions taken with original and new paths
   - Items skipped and why
   - A reversibility script (shell commands to undo all moves)

8. **Commit the log** to the repo.
