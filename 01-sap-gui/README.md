# 01 — SAP GUI Automation

SAP GUI is a good example of why UI automation should start by identifying the application's most stable automation layer.

## Typical challenges

- Dynamic SAP session/window identifiers
- Grid and table controls with changing indexes
- Nested controls under tabs and containers
- Confirmation and modal dialogs
- UI elements that are visible but difficult to target reliably

## Recommended approach

Use the most application-aware and stable mechanism available in the environment. Standard PAD UI element capture can work well for many controls; SAP GUI scripting or other SAP-aware mechanisms may be preferable when the target exposes a stable semantic object model and the environment permits it.

Do not assume one technique is always better. Validate the approach against application behavior, governance requirements, and maintainability.

## Grid targeting pattern

A resilient approach for a SAP grid is:

1. Identify the stable parent/container.
2. Identify the grid control.
3. Avoid relying on volatile session identifiers where possible.
4. Use stable grid properties or indexes only where the index is known to be deterministic.
5. Validate that the expected row/column or cell is actually selected.
6. Handle confirmation dialogs explicitly.

Example selector fragments may look similar to:

```text
/usr/tabs.../shell[...]
```

The exact hierarchy varies by SAP transaction, screen, GUI version, and configuration. Do not copy a selector blindly between environments.

## Dynamic session IDs

If a selector contains a value that changes between sessions, first determine whether the property is actually required for uniqueness. If another stable property can identify the control, prefer that property.

Use regex only when it solves a demonstrated volatility problem. Overusing regex can make a selector less predictable rather than more resilient.

## Modal dialogs

Treat dialogs as part of the business interaction, not as an unexpected exception.

A robust flow should:

- Perform the action.
- Wait for the expected dialog state where appropriate.
- Read or validate the dialog.
- Take the required action.
- Confirm that the original operation completed.

Avoid long unconditional delays when a state-based wait is available.

## Troubleshooting checklist

| Symptom | Check first |
|---|---|
| Element cannot be found | Re-capture and inspect the selector hierarchy |
| Works in one session only | Look for volatile session/window properties |
| Grid row changes | Check whether row index is deterministic |
| Click works but action does not complete | Validate focus and post-action state |
| Dialog appears intermittently | Add state-based detection and controlled handling |

## Common mistakes

- Copying a selector from one SAP environment without validating it.
- Treating every changing attribute as a problem that requires regex.
- Using absolute coordinates when a stable application-level target exists.
- Assuming a successful click means the business operation succeeded.
- Using long fixed delays instead of waiting for a meaningful state.

## Implementation file

A runnable `.pad` example will be added once the SAP grid-selection recipe is finalized and validated in a representative SAP GUI environment.
