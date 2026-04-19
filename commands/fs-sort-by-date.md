Sort files in a directory into subdirectories by date.

Ask the user for a target directory if not provided as $ARGUMENTS.

1. **Load guardrails** from `guardrails/`.

2. **Catalogue files** by modification date.

3. **Propose organisation** into date-based folders using the project's date hierarchy convention (two-digit year → two-digit month → two-digit day):

   - **Year only**: `25/`, `24/`
   - **Year/Month** (default): `25/10/`, `25/11/`
   - **Year/Month/Day**: `25/10/29/`

   Ask the user which granularity they prefer. Default to Year/Month if not specified.

   For archive/backup directories, use four-digit years with optional quarterly subdivisions:
   ```
   archive/
   ├── 2024/
   │   ├── q1/
   │   └── q2/
   └── 2023/
   ```

4. **Wait for user approval**.

5. **Execute and log** to `logs/`.
