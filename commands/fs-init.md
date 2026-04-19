Run this at the start of every conversation to load workspace state.

1. **Check guardrails**: Read all files in `guardrails/` (excluding `.gitkeep` and the example). List any active guardrails found. If none exist, inform the user and ask if they'd like to set any up before proceeding.

2. **Check scan data**: Read all files in `data/`. Summarise the most recent scans — which directories were scanned, when, and what the clutter scores were. If no scans exist, note this.

3. **Check logs**: Read the most recent files in `logs/`. Summarise the last few cleanup operations — what was done, when, and where. If no logs exist, note this.

4. **Report status**: Present a brief summary:
   - Number of active guardrails
   - Most recent scan (directory + date) or "No scans yet"
   - Most recent cleanup (directory + date) or "No cleanups yet"

5. **Prompt for action**: Ask the user what they'd like to do next — scan for clutter, clean a directory, check status, or set up guardrails.
