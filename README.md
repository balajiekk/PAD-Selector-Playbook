# PAD-Selector-Playbook

A curated collection of practical patterns, recipes, and templates for handling notoriously tricky UI automation scenarios in **Microsoft Power Automate Desktop (PAD)**.

The playbook focuses on the situations where standard UI element capture is not enough and the automation needs a more deliberate targeting, fallback, or recovery strategy.

> **Status:** Documentation and implementation patterns are being built first. Runnable `.pad` examples will be added as the implementation examples are finalized and validated.

---

## 🎯 What's Inside

| Module | Focus Area | Key Techniques |
| :--- | :--- | :--- |
| [01-sap-gui](./01-sap-gui/) | Enterprise ERP Automation | SAP GUI scripting, dynamic session handles, shell table grid navigation |
| [02-citrix-surface-automation](./02-citrix-surface-automation/) | Virtualized / Remote Desktops | Bounded subregion OCR, image tolerance tuning, visual anchor targeting |
| [03-dynamic-web-tables](./03-dynamic-web-tables/) | Web Grids & Pagination | Variable injection (`%Var%`), regex matching (`eq\(\d+\)`), resilient pager loops |
| [04-canvas-and-custom-controls](./04-canvas-and-custom-controls/) | Non-Standard UI Elements | Anchor-based sibling targeting, multi-selector fallback hierarchies |
| [05-nested-iframes](./05-nested-iframes/) | Embedded Web Content | Frame-aware targeting, cross-domain limitations, JavaScript bridge patterns |
| [06-shadow-dom-saas](./06-shadow-dom-saas/) | Salesforce / ServiceNow / Modern SaaS | Shadow DOM awareness, JavaScript-based interaction patterns |
| [07-unpredictable-modals](./07-unpredictable-modals/) | Random Popups & Dialogs | Proactive window-state checks, controlled error recovery, popup dismissal |
| [cheatsheets](./cheatsheets/) | Quick Reference | Regex operators, selector syntax, UI properties, recovery and decision rules |

---

## Selector Resilience Hierarchy

Use the strongest and most stable automation layer available. Do not jump directly to image recognition or coordinates simply because a selector is difficult.

1. Application/API/native automation
2. Stable UI properties
3. Relative or anchor-based targeting
4. Dynamic selector properties with carefully scoped regex
5. OCR / text matching
6. Image recognition
7. Absolute coordinates — last resort

The objective is not to create the shortest selector. The objective is to create an automation that survives predictable application changes.

---

## Repository Structure

```text
PAD-Selector-Playbook/
├── README.md
├── CONTRIBUTING.md
├── 01-sap-gui/
│   └── README.md
├── 02-citrix-surface-automation/
│   └── README.md
├── 03-dynamic-web-tables/
│   └── README.md
├── 04-canvas-and-custom-controls/
│   └── README.md
├── 05-nested-iframes/
│   └── README.md
├── 06-shadow-dom-saas/
│   └── README.md
├── 07-unpredictable-modals/
│   └── README.md
└── cheatsheets/
    ├── selector-operators-regex.md
    ├── ui-element-properties.md
    └── automation-decision-tree.md
```

Runnable PAD flow files will be added beside the relevant recipe once each example has been tested.

---

## Recommended Recipe Format

Each scenario follows the same structure:

**Problem → Why the normal approach fails → Resolution → Implementation pattern → Fallback → Validation → Common mistakes**

This keeps the repository useful as both a learning resource and a practical reference during development and troubleshooting.

---

## Prerequisites

- Microsoft Power Automate Desktop
- A test application/environment appropriate for the scenario
- Permission to automate the target application
- For Citrix/VDI scenarios, a stable test screen and awareness of display scaling/DPI settings
- For SAP GUI scenarios, appropriate SAP GUI configuration and scripting permissions where required
- For JavaScript-based web recipes, a browser session and a target page where the relevant DOM APIs are accessible

## Important Note

Selectors, UI properties, JavaScript behavior, image recognition thresholds, OCR results, and application controls can vary by application version, browser, Windows configuration, display scaling, and environment. Treat the examples as patterns rather than universal values.

---

## Goal

The goal of this playbook is simple:

> **Make PAD UI automation more resilient, predictable, and easier to troubleshoot.**

If a UI automation breaks, the first question should not be *"How do I make the selector work?"* It should be *"What is the most stable automation layer available for this interaction?"*
