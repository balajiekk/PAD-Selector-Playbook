# Automation Decision Tree

Use this decision tree before choosing a selector, OCR, image recognition, or coordinates.

```text
Can the application expose a native/API/business-level operation?
            │
        ┌───Yes───┐
        ↓         ↓
      Prefer it   No
                  ↓
      Is a stable UI/web element exposed?
                  │
             ┌────Yes────┐
             ↓           ↓
           Use it        No
                         ↓
          Can a stable anchor identify the target?
                         │
                    ┌────Yes────┐
                    ↓           ↓
             Relative targeting No
                                ↓
                   Can text identify the target?
                                │
                           ┌────Yes────┐
                           ↓           ↓
                         OCR/text     No
                                       ↓
                         Can a distinctive image identify it?
                                       │
                                  ┌────Yes────┐
                                  ↓           ↓
                              Image match    No
                                              ↓
                                      Coordinates only
                                      as last resort
```

## Selection rules

### Prefer native/API automation

Use it when the application provides a stable and supported business/application interface.

### Prefer stable UI properties

Use stable names, types, relationships, and meaningful attributes exposed by the target application.

### Use dynamic matching carefully

If a property is volatile but follows a predictable pattern, use a narrowly scoped dynamic rule or regex.

### Use visual techniques when necessary

OCR and image recognition are appropriate when the automation surface is visual, such as some Citrix/VDI and canvas scenarios.

### Coordinates are the last resort

Coordinates should be used only when the environment can be sufficiently controlled and no stronger targeting mechanism exists.

## Final question

Before committing an automation approach, ask:

> **If the application changes slightly tomorrow, which part of this automation is most likely to break?**

Design around that answer.
