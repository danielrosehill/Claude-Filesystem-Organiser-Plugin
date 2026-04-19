# Example Guardrail

This is an example guardrail file. Create your own guardrail files in this directory to define rules that override the general cleanup instructions.

## Format

Each guardrail file should contain:

- **Rule name**: A short, descriptive title
- **Type**: One of `exclude`, `permit-loose`, `nesting-rule`, `protected-pattern`
- **Scope**: Which directories or file patterns this applies to
- **Rule**: What the agent should do (or not do)

## Examples

### Exclude a directory
- **Type**: exclude
- **Scope**: `~/Projects/active-client-work/`
- **Rule**: Never touch anything in this directory tree.

### Permit loose files
- **Type**: permit-loose
- **Scope**: `~/Desktop/`
- **Rule**: Allow `.sh` scripts to remain loose on the Desktop.

### Custom nesting rule
- **Type**: nesting-rule
- **Scope**: `~/Documents/invoices/`
- **Rule**: Nest invoices by year, then by month (`2024/03/`).

### Protected pattern
- **Type**: protected-pattern
- **Scope**: `**/*.desktop`
- **Rule**: Never move or rename `.desktop` files anywhere on the system.
