# 05 — Nested IFrames

Embedded web content is one of the common reasons a browser automation that works on the main page suddenly reports **Element not found** for a perfectly visible control.

## The Bottleneck

A target element may be hosted inside an `<iframe>` rather than the top-level document. With nested or cross-origin frames, the element belongs to a different document context.

Typical examples include:

- Payment gateway widgets
- Embedded forms
- CRM widgets
- Partner portals
- Authentication components

The important distinction is that an iframe is not simply another HTML container. It creates a separate browsing context.

## Resolution Strategy

### 1. Identify the frame hierarchy

First determine whether the target is:

```text
Top document
   └── iframe
       └── nested iframe
           └── target element
```

If the element is several frames deep, every relevant frame boundary needs to be considered.

### 2. Prefer direct navigation when appropriate

If the iframe source URL is independently accessible and direct navigation is safe for the business process, switching browser context or navigating directly to the source can simplify the automation.

This should only be used when the application supports it. Authentication, session state, referrer checks, tokens, and security controls may make direct navigation unsuitable.

### 3. JavaScript bridge

For pages where JavaScript access is permitted, PAD's **Run JavaScript function on web page** action can be used to inspect or interact with accessible DOM content.

A conceptual same-origin example is:

```javascript
const frame = document.getElementById('frameId');
const frameDocument = frame.contentWindow.document;
const element = frameDocument.querySelector('.target');
```

The exact implementation depends on the page structure.

## Important cross-domain limitation

JavaScript cannot simply bypass browser same-origin security. If an iframe is genuinely cross-origin, accessing its `contentWindow.document` from the parent page is normally blocked by the browser's security model.

Therefore, do **not** position JavaScript injection as a universal way to pierce cross-domain iframe boundaries.

Possible alternatives include:

- Automating the frame as its own browser context when PAD/browser capabilities allow it
- Navigating to an independently accessible source page where appropriate
- Using an application/API integration instead of UI automation
- Using an exposed message/API mechanism designed by the application

## Nested iframe troubleshooting

| Symptom | Likely cause | First check |
|---|---|---|
| Main-page elements work, iframe target fails | Wrong document context | Inspect iframe hierarchy |
| First iframe works, nested target fails | Additional frame boundary | Inspect nested frames |
| JavaScript throws security error | Cross-origin frame | Check frame origin |
| Direct URL works but business flow fails | Session/authentication dependency | Validate cookies/session state |

## Common mistakes

- Assuming every visible element belongs to the top document.
- Treating `iframe` as a normal DOM container.
- Assuming JavaScript can bypass cross-origin restrictions.
- Navigating directly to an iframe URL without validating session/authentication dependencies.
- Using brittle frame indexes instead of stable frame identifiers where available.

## Implementation file

A runnable nested-iframe example will be added after the target application and frame structure are selected and the implementation is validated.
