# Verification report format

Lead with the documentation outcome, not the browser actions.

## Summary

- **Confirmed:** Claims that match the tested UI.
- **Corrections needed:** Claims contradicted by direct evidence.
- **Blocked:** Claims that could not be tested and the exact prerequisite.
- **Needs corroboration:** Claims that require API output, execution results, release information, or another authoritative source.

## Claim evidence

For each material claim, include:

- Claim ID and concise wording.
- Result: confirmed, contradicted, partially confirmed, blocked, or needs corroboration.
- Environment and relevant account, organization, project, or feature state.
- Evidence artifact names.
- Documentation action taken or recommended.

## Documentation changes

List only edits supported by the evidence. Separate:

- Exact UI terminology corrections.
- Flow or ordering corrections.
- Missing prerequisites or branch conditions.
- Claims removed because they exceed the evidence.
- Screenshots added or replaced, and the prose each one let you delete.

Call out separately anything you verified as already correct and therefore did not touch, especially where you were close to changing it. That is what stops the same line being rewritten back and forth across sessions.

## Remaining uncertainty

State what was not established. Do not turn a blocked check into a product limitation.

When screenshots materially help, embed only the relevant images in the response. Do not expose unrelated tabs, account data, identifiers, or secrets.
