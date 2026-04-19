Help the user create a new guardrail and save it to the `guardrails/` directory.

Ask the user to describe the rule they want. Then create a guardrail file with the following structure:

```markdown
# [Rule Name]

- **Type**: One of `exclude`, `permit-loose`, `nesting-rule`, `protected-pattern`
- **Scope**: Which directories or file patterns this applies to
- **Rule**: What the agent should do (or not do)
```

Save the file to `guardrails/` with a descriptive kebab-case filename (e.g., `exclude-active-projects.md`, `permit-desktop-scripts.md`).

Confirm the guardrail was saved and remind the user it will be loaded automatically on `/init`.
