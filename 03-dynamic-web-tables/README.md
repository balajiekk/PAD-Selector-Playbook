# 03 — Dynamic Web Tables

Web applications frequently render tables where some selector properties change between rows, sessions, or page loads.

The goal is not to make every attribute dynamic. The goal is to identify which properties are stable and use dynamic matching only where necessary.

## Stable vs dynamic properties

Before changing a selector, inspect the captured element and classify its properties:

| Property type | Guidance |
|---|---|
| Semantic role/type | Prefer when stable |
| Accessible name/text | Prefer when meaningful and stable |
| Container/parent relationship | Useful for scoping |
| Static IDs/classes | Prefer when genuinely stable |
| Generated IDs/classes | Treat as volatile |
| Row indexes | Use only when deterministic |

## Regex for volatile attributes

If a property contains a predictable dynamic portion, a carefully scoped regex can be useful.

For example, a generated attribute may resemble:

```text
document:eq(17)
document:eq(18)
document:eq(19)
```

A conceptual regex pattern could match the numeric portion:

```text
document:eq\(\d+\)
```

The exact selector syntax and escaping should be validated in PAD's selector editor. Regex should be applied only to the property that is known to vary.

## PAD variables vs regex

These are different mechanisms:

- `%RowIndex%` is a PAD variable used to substitute a runtime value.
- `%InvoiceNumber%` is a PAD variable used to insert a business value.
- Regex is a matching rule used to tolerate a predictable pattern in a target property.

Do not treat PAD variable substitution and regex as interchangeable.

## Row matching pattern

For business-driven row selection:

```text
Read/identify business key
        ↓
Locate matching row
        ↓
Validate row contents
        ↓
Interact with target cell/control
        ↓
Validate post-action state
```

Where possible, match on a meaningful business key such as invoice number, order number, customer reference, or status instead of a visual row position.

## Pagination pattern

A robust pagination loop should explicitly handle:

- Current page processing
- Next-button availability
- Disabled Next button
- End-of-results state
- Page-change validation
- Maximum page/iteration guard

Conceptually:

```text
Process current page
        ↓
Is Next available and enabled?
   ┌────┴────┐
  No        Yes
   ↓          ↓
  End     Click Next
              ↓
       Wait for page change
              ↓
       Validate page changed
              ↓
       Continue
```

### Preventing infinite loops

After clicking Next, verify that the page actually changed. A disabled or ineffective Next action must not cause the same page to be processed indefinitely.

A maximum iteration/page guard is also recommended for defensive automation.

## Common mistakes

- Applying regex to every selector property.
- Matching rows by position when a business key is available.
- Assuming a Next button is enabled because it exists.
- Clicking Next without confirming the page changed.
- Relying on dynamic CSS classes without understanding their lifecycle.
- Processing a page successfully but never validating that the intended row was selected.

## Implementation files

Runnable examples for regex-based row matching and pagination will be added after the recipes are finalized and validated.
