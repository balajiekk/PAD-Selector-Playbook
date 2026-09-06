# PAD Selector Playbook

A practical playbook for building reliable and maintainable UI automation with **Microsoft Power Automate Desktop (PAD)**.

This repository focuses on the UI automation scenarios where standard UI element capture is not enough: SAP GUI grids, Citrix/VDI surfaces, dynamic web tables, canvas/custom controls, and recovery patterns.

> **Status:** Documentation and implementation patterns are being built first. Runnable `.pad` examples will be added as the implementation examples are finalized and validated.

## What this playbook covers

- **SAP GUI** – choosing between SAP-aware/native targeting, UI element properties, scripting-assisted approaches, grids, dynamic IDs, and modal dialogs.
- **Citrix / VDI** – surface automation, image recognition, OCR, visual anchors, bounded search regions, and recovery strategies.
- **Dynamic web tables** – stable attributes, dynamic selector properties, regex, row matching, and pagination.
- **Canvas and custom controls** – anchor-based targeting, relative interaction, validation, and fallback selectors.
- **Error handling and recovery** – retries, validation, screenshots, controlled fallbacks, and avoiding infinite loops.
- **Cheatsheets** – selector operators, regex patterns, UI properties, and an automation decision tree.

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

## Repository structure

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
├── 05-error-handling-and-recovery/
│   └── README.md
└── cheatsheets/
    ├── selector-operators-regex.md
    ├── ui-element-properties.md
    └── automation-decision-tree.md
```

Runnable PAD flow files will be added beside the relevant recipe once each example has been tested.

## Recommended recipe format

Each scenario follows the same structure:

**Problem → Why the normal approach fails → Recommended approach → Implementation pattern → Fallback → Validation → Common mistakes**

This keeps the repository useful as both a learning resource and a practical reference during development and troubleshooting.

## Prerequisites

- Microsoft Power Automate Desktop
- A test application/environment appropriate for the scenario
- Permission to automate the target application
- For Citrix/VDI scenarios, a stable test screen and awareness of display scaling/DPI settings
- For SAP GUI scenarios, the appropriate SAP GUI configuration and scripting permissions where required

## Important note

Selectors, UI properties, image recognition thresholds, OCR behavior, and application controls can vary by application version, Windows configuration, display scaling, and environment. Treat the examples as patterns rather than universal values.

## Goal

The goal of this playbook is simple:

> **Make PAD UI automation more resilient, predictable, and easier to troubleshoot.**

If a UI automation breaks, the first question should not be *"How do I make the selector work?"* It should be *"What is the most stable automation layer available for this interaction?"*
