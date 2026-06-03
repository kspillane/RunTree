---
layout: default
title: Documentation
permalink: /docs
---

<div class="prose max-w-3xl mx-auto py-4">

# RunTree Documentation

RunTree is an open-source, interactive troubleshooting engine designed to replace static wiki runbooks. It generates step-by-step diagnostic decision trees from flat, easy-to-read YAML configurations.

By utilizing a state-machine architecture, RunTree allows engineers to deep-link to specific troubleshooting steps, view their diagnostic path via breadcrumbs, and resolve incidents faster.

---

## The YAML Schema Design

RunTree uses a **flat state-machine layout**. Instead of nesting steps inside one another, every diagnostic step is defined as a distinct node with a unique ID inside a top-level `steps` mapping. You navigate between steps by defining options that target these step IDs.

Here is a complete, copy-pasteable example file:

```yaml
metadata:
  id: "atm-dispenser-fault"
  title: "ATM Dispenser Fault"
  description: "Diagnostic flow for cash dispenser hardware errors."
steps:
  start:
    title: "Verify Error State"
    body: "Check the ATM monitoring dashboard."
    options:
      - label: "Shows Bill Jam"
        target: "check_physical_jam"
        type: "primary"
      - label: "View KCS Article"
        url: "https://kb.internal/123"
        type: "help"
  check_physical_jam:
    title: "Check for Physical Jam"
    body: "Open the vault and check the transport path."
    options:
      - label: "Jam Cleared"
        target: "resolved"
        type: "success"
      - label: "No Jam"
        target: "dispatch_vendor"
        type: "danger"
  resolved:
    title: "Resolved"
    body: "Document steps and close ticket."
    options: []
  dispatch_vendor:
    title: "Dispatch Vendor"
    body: "Open a ticket with the hardware vendor."
    options: []
```

---

## Field Specifications

### 1. Metadata Block
The top-level `metadata` block registers the flow:
* `id` *(String, Required)*: The unique URL slug for this flow (e.g. `atm-dispenser-fault`).
* `title` *(String, Required)*: The human-readable name of the runbook.
* `description` *(String, Optional)*: A short summary of what this diagnostic flow solves.

### 2. Steps Block
The `steps` map contains all nodes. 
* Every flow **must** have a step key named `start`. This is the entry point when a user opens the runbook.
* Each step has:
  * `title` *(String)*: Title of the step.
  * `body` *(String)*: Actionable instructions. Bold text (`**text**`) and markdown links (`[Label](URL)`) are supported.
  * `options` *(Array)*: Branching buttons for the user to select.

### 3. Option Configuration
Options within a step allow user interaction. Each option MUST have either a `target` OR a `url`:
* `label` *(String)*: The text displayed on the button.
* `target` *(String)*: The key/ID of another step to navigate to (e.g., `check_physical_jam`).
* `url` *(String)*: An external link (e.g., KCS knowledge base, monitoring console). If specified, clicking the button will open this URL in a new browser tab.
* `type` *(String)*: Sets the button style. Available types:
  * `primary` - Blue background (standard actions).
  * `success` - Green background (resolutions/clearing checks).
  * `danger` - Red background (escalations/error pathways).
  * `help` - Outlined styling with an external link indicator.

---

## Authoring Guide for New Flows

1. Create a new `.yml` file in the `_data/flows/` directory (e.g., `_data/flows/database-cpu-alert.yml`).
2. Add your `metadata` details at the top.
3. Map out your decision nodes. Sketch the flowchart beforehand to identify the step keys.
4. Ensure the entry point is named `start`.
5. Connect your nodes by linking options with their corresponding `target` ID.
6. Verify your changes. Since Jekyll runs in a development container, saving the YAML file will immediately compile and display the new runbook on your dashboard.

</div>
