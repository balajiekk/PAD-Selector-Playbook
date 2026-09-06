# 06 — Shadow DOM in Modern SaaS

Modern SaaS applications increasingly use Web Components and Shadow DOM to encapsulate component markup and styling. This can make ordinary DOM selectors appear to stop at a `#shadow-root` boundary.

Common examples include enterprise applications such as Salesforce Lightning, ServiceNow, and other component-based SaaS platforms.

## The Bottleneck

A simplified structure may look like:

```text
Document
└── my-custom-element
    └── #shadow-root
        └── input.inner-field
```

A selector operating only in the light DOM may not be able to reach the nested element in the way expected.

## Resolution Strategy

For a specific control that is inaccessible through standard PAD web/UI targeting, JavaScript can be considered when the page context and browser security model permit it.

Conceptually:

```javascript
const host = document.querySelector('my-custom-element');
const input = host?.shadowRoot?.querySelector('.inner-input');

if (input) {
    input.value = 'Data';
    input.dispatchEvent(new Event('input', { bubbles: true }));
    input.dispatchEvent(new Event('change', { bubbles: true }));
}
```

The actual events required by a framework vary. Some frameworks require property setters, composed events, or component-specific APIs rather than direct `.value` assignment.

## PAD approach

The pattern is:

1. Identify the Shadow DOM host.
2. Determine whether the shadow root is accessible from the page context.
3. Use **Run JavaScript function on web page** for the specific interaction where appropriate.
4. Trigger the events expected by the application.
5. Validate the application state after the script runs.

Do not replace all PAD browser automation with JavaScript. Use it selectively for controls that genuinely require it.

## Open vs closed Shadow DOM

JavaScript access depends on how the component is implemented. An **open** shadow root can expose `shadowRoot` to page scripts. A **closed** shadow root does not expose the same direct reference.

A script such as:

```javascript
host.shadowRoot
```

therefore cannot be assumed to work for every component.

## Framework considerations

Modern SaaS applications may use frameworks that maintain internal state. Directly changing an input's DOM value may not update the framework's state correctly.

When possible, prefer:

- Supported application APIs
- Accessible UI interactions
- Component-specific behavior
- Events that the application actually listens for

## Common mistakes

- Assuming every `#shadow-root` is accessible through `shadowRoot`.
- Setting `.value` without firing the events required by the application.
- Assuming DOM state and framework state are always identical.
- Using JavaScript when a stable native/UI automation path already exists.
- Skipping post-script validation.

## Implementation file

A runnable SaaS Shadow DOM example will be added after a representative non-production target is selected and the JavaScript interaction is validated.
