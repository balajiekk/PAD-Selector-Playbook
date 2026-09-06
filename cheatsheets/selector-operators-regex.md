# Selector Operators & Regex Cheatsheet

A quick reference for designing resilient PAD selectors. Always validate the exact operator and syntax supported by the current PAD selector editor.

## Selector principles

- Prefer stable properties over volatile generated values.
- Scope a target under a stable parent when possible.
- Use exact matching when the value is stable.
- Use pattern matching only for demonstrated volatility.
- Avoid making the entire selector dynamic when only one property changes.

## Regex building blocks

| Pattern | Meaning |
|---|---|
| `\d+` | One or more digits |
| `\d{2,4}` | Two to four digits |
| `[A-Za-z]+` | One or more letters |
| `\s+` | One or more whitespace characters |
| `.*` | Any sequence of characters — use carefully |
| `^...$` | Match the complete value |
| `\(` / `\)` | Literal parentheses |

## Example

For a predictable generated value such as:

```text
document:eq(17)
document:eq(18)
document:eq(19)
```

A conceptual pattern is:

```text
document:eq\(\d+\)
```

## PAD variables

Runtime substitution is different from regex matching:

```text
%RowIndex%
%InvoiceNumber%
%CustomerId%
```

Use a PAD variable when the target value is known at runtime. Use regex when a selector property follows a predictable pattern but the exact value is not known.

## Warning

`.*` is powerful but broad. Prefer the narrowest pattern that solves the actual volatility problem.
