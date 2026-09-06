# 07 — Unpredictable Modals & Popups

Unexpected dialogs are one of the simplest ways to make an otherwise reliable UI automation fail. A popup can steal focus, block the target, or change the active window at exactly the wrong time.

Typical examples include:

- Software update notifications
- "Rate this software" prompts
- Promotional overlays
- Session timeout dialogs
- Browser permission prompts
- Unexpected confirmation windows

## The Bottleneck

A critical sequence may expect:

```text
Target page → Target control → Action
```

but an unexpected popup changes it to:

```text
Target page → Popup → Target control is blocked
```

The selector may still be correct. The application state is not.

## Resolution Strategy

### 1. Detect expected state proactively

Before important interactions, validate that the expected window/page/control is available.

### 2. Use controlled error handling

For a fragile interaction sequence, PAD's **On block error** handling can route execution to a recovery path when an interaction fails.

Depending on the design, the recovery path can:

- Go to a label
- Run a subflow
- Perform a controlled retry
- Capture diagnostics
- Stop with a meaningful error

### 3. Detect the rogue modal

A recovery subflow can check for conditions such as:

- **If window exists**
- **If web page contains**
- A known UI element
- A known title/text pattern

If the popup is confirmed, close it and validate that the original application state has returned.

## Recovery pattern

```text
Critical UI action
       ↓
Action succeeds?
  ┌────┴────┐
 Yes        No
  ↓          ↓
Continue   Recovery subflow
              ↓
       Is known popup present?
          ┌───┴───┐
         Yes      No
          ↓        ↓
       Close     Capture evidence
       popup        ↓
          ↓      Controlled failure
    Validate state
          ↓
       Retry once/bounded
          ↓
       Continue
```

## Why not use a permanent popup loop?

A recovery routine should not blindly search for and close windows forever. The popup may be legitimate, or the actual failure may have a different cause.

Use:

- A bounded retry count
- Known popup signatures
- State validation
- Clear exit conditions

## Parallel monitoring — use carefully

Some architectures may monitor application state while the primary automation runs. This can be useful for known, genuinely asynchronous popups, but it also increases complexity and race-condition risk.

Prefer a simple proactive check or controlled recovery path unless asynchronous monitoring is justified by the application behavior.

## Common mistakes

- Assuming every failure is caused by a popup.
- Closing any window whose title happens to match a broad pattern.
- Retrying without checking whether the popup was actually removed.
- Using unlimited retries.
- Resuming the main flow without validating application state.
- Capturing sensitive popup content in logs or screenshots without considering data-protection requirements.

## Implementation file

A reusable unpredictable-modal recovery template will be added once the target popup scenario is finalized and validated.
