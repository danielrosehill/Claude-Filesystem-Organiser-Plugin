Flatten an over-nested directory structure.

Sometimes directories are too deeply nested with single-item folders. This command identifies and collapses unnecessary nesting levels.

Ask the user for a target directory if not provided as $ARGUMENTS.

1. **Load guardrails** from `guardrails/`.

2. **Identify over-nesting**:
   - Folders containing only one subfolder and nothing else
   - Chains of single-child directories (e.g., `a/b/c/` where each has only one child)
   - Empty intermediate directories

3. **Propose flattening**: Show which folders would be collapsed and what the new structure would look like.

4. **Wait for user approval**.

5. **Execute and log** to `logs/`.
