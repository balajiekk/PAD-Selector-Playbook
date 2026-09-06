# 05 — Error Handling and Recovery

Reliable UI automation is not only about finding a control. It is also about detecting when an interaction did not produce the expected result and recovering safely.

## Recovery pattern

Use the following structure for fragile UI interactions:

```text
Prepare expected state
        ↓
Interact
        ↓
Validate result
   ┌────┴────┐
 Success    Failure
    ↓          ↓
 Continue   Diagnose
               ↓
          Controlled retry
               ↓
          Alternate strategy
               ↓
          Fail with evidence
```

## Recommended controls

### 1. State validation

Before interacting, confirm the application is in the expected state where practical.

### 2. Post-action validation

After interacting, verify a meaningful outcome instead of assuming the action succeeded.

### 3. Bounded retries

Retries should have a clear maximum. An unlimited retry is usually an infinite loop with a different name.

### 4. Progressive fallback

Move from a stronger technique to a weaker technique only when the stronger technique fails for a known reason.

### 5. Diagnostic evidence

For difficult UI failures, capture useful evidence such as screenshots, target details, current page/state, and error information according to organizational policy.

## Avoid fixed-delay recovery

A long delay may hide timing problems without solving them. Prefer waiting for a meaningful state, such as:

- Window available
- Element exists
- Loading indicator disappears
- Expected text appears
- Button becomes enabled
- Target state changes

## Recovery decision example

```text
Target not found
      ↓
Is application state correct?
  ├─ No → restore expected state → retry
  └─ Yes
       ↓
Is another stable selector available?
  ├─ Yes → use fallback selector → validate
  └─ No
       ↓
Can OCR/image/anchor targeting be used safely?
  ├─ Yes → controlled visual fallback → validate
  └─ No → fail with diagnostic evidence
```

## Infinite-loop protection

Any loop involving UI state should have at least one defensive condition:

- Maximum attempts
- Maximum pages
- Timeout
- State-change validation
- Progress counter

## Common mistakes

- Retrying without changing anything.
- Treating an exception as proof that the business transaction failed.
- Continuing after an unvalidated click.
- Using large fixed delays everywhere.
- Building fallbacks without defining when they should stop.
- Logging sensitive screen content without considering data-protection requirements.

## Implementation file

A reusable recovery template will be added after the implementation pattern is finalized and validated.
