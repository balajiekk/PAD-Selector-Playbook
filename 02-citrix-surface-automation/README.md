# 02 — Citrix / VDI Surface Automation

Citrix and VDI environments can expose only the rendered screen to PAD. When native UI automation is unavailable, surface automation becomes a practical fallback.

## Preferred hierarchy

1. Native/application/API automation, when available
2. PAD UI/web automation, when the target is exposed
3. Image recognition or OCR for surface-only interactions
4. Absolute coordinates only as a last resort

## Image recognition pattern

For a visual target:

1. Identify a distinctive visual anchor.
2. Restrict the search to a bounded screen region where practical.
3. Tune recognition tolerance using representative test cases.
4. Click relative to the detected anchor rather than using a fixed screen coordinate.
5. Validate the resulting state.
6. Recover or retry in a controlled manner when the target is not found.

A bounded region can be represented conceptually as:

```text
(X1, Y1, X2, Y2)
```

The actual values should be environment-specific.

## OCR pattern

OCR can be useful when the required target is identifiable by text rather than appearance.

Example approach:

```text
Capture bounded region
        ↓
Extract text with OCR
        ↓
Normalize/compare text
        ↓
Locate or calculate interaction target
        ↓
Interact
        ↓
Validate result
```

## Display scaling and resolution

Surface automation is sensitive to:

- Screen resolution
- Windows display scaling/DPI
- Remote session size
- Application zoom level
- Font rendering
- Citrix display policies

A flow tested at 100% scaling may behave differently at another scaling level. Record the supported display configuration as part of the recipe.

## Image recognition tuning

Do not start by increasing tolerance until the image matches everything on screen. Instead:

- Choose a distinctive anchor.
- Keep the captured image small and specific.
- Search within a sensible region.
- Test normal and slightly changed states.
- Record the tested tolerance and environment.

## Relative click pattern

Absolute coordinates are fragile because the target moves when the window or screen changes.

A stronger pattern is:

```text
Find visual anchor
      ↓
Get anchor location
      ↓
Apply relative X/Y offset
      ↓
Click
      ↓
Validate expected state
```

## Recovery

If the visual target is not found:

- Confirm the application is in the expected state.
- Re-check the bounded region.
- Capture a diagnostic screenshot where appropriate.
- Retry only a bounded number of times.
- Fall back to an alternate anchor or OCR strategy if available.
- Fail with a useful diagnostic message rather than continuing blindly.

## Common mistakes

- Using full-screen image searches when a smaller region is available.
- Depending on exact coordinates.
- Ignoring display scaling.
- Using a visually common image as the anchor.
- Increasing tolerance until false positives appear.
- Clicking without validating the resulting application state.

## Implementation file

The runnable image-fallback template will be added after the recipe is finalized and tested against a representative Citrix/VDI scenario.
