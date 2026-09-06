# 04 — Canvas and Custom Controls

Canvas-based interfaces and custom controls can be difficult to automate because the rendered object may expose little or no useful DOM/UI metadata.

## Anchor-based targeting

When a nearby stable element is available, use it as an anchor rather than depending on a fixed coordinate.

Recommended pattern:

```text
Find stable anchor
        ↓
Determine relative position
        ↓
Interact with target
        ↓
Validate expected result
```

The anchor should be visually and functionally close to the target and should remain stable across expected application states.

## Multi-selector fallback hierarchy

For controls that may expose different properties in different states, define a deliberate fallback hierarchy rather than changing selectors ad hoc during troubleshooting.

Example:

```text
Primary: stable UI property
        ↓
Fallback: relative/anchor target
        ↓
Fallback: OCR/text match
        ↓
Last resort: image recognition
```

Each fallback should include validation so that the flow does not continue after an incorrect interaction.

## Canvas interaction considerations

Canvas controls may depend on:

- Window position
- Screen resolution
- Display scaling
- Application zoom
- Browser zoom
- Canvas scrolling
- Rendering timing
- Hover state

Record these environmental assumptions with the implementation example.

## Validation is mandatory

A successful click does not prove that the intended canvas object was activated.

After interaction, validate an observable state such as:

- Selection highlight
- Changed label/value
- Opened panel
- Changed focus/state
- Expected downstream control becoming available

## Common mistakes

- Using absolute coordinates without controlling the environment.
- Choosing an unstable visual anchor.
- Ignoring browser/application zoom.
- Skipping post-click validation.
- Adding many fallback methods without defining their order or stopping conditions.

## Implementation file

The anchor-based targeting example will be added once the implementation is finalized and validated against a representative custom-control scenario.
