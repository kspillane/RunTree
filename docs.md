---
layout: default
title: Documentation
permalink: /docs
---

<div class="prose max-w-3xl mx-auto py-4" markdown="1">

# RunTree Documentation

RunTree is an open-source, interactive troubleshooting engine designed to replace static wiki runbooks. It generates step-by-step diagnostic decision trees from flat, easy-to-read YAML configurations.

By utilizing a state-machine architecture, RunTree allows engineers to deep-link to specific troubleshooting steps, view their diagnostic path via breadcrumbs, and resolve incidents faster.

---

## The YAML Schema Design

RunTree uses a **flat state-machine layout**. Instead of nesting steps inside one another, every diagnostic step is defined as a distinct node with a unique ID inside a top-level `steps` mapping. You navigate between steps by defining options that target these step IDs.

Here is a complete, copy-pasteable example file:

```yaml
---
metadata:
  id: "browser-offline"
  title: "Browser Showing Offline"
  description: "Diagnostic flow for when a browser shows offline."
  type: "Network Connectivity Issues"
steps:
  start:
    title: "Check Physical Connection"
    body: "Check the network cable."
    options:
      - label: "Connected properly"
        target: "check_ip"
        type: "primary"
      - label: "Not connected"
        target: "fix_physical"
        type: "danger"
  check_ip:
    title: "Check IP"
    body: "Run ipconfig."
    options:
      - label: "Valid IP"
        target: "resolved"
        type: "success"
  fix_physical:
    title: "Plug it in"
    body: "Plug in the cable."
    options:
      - label: "Done"
        target: "resolved"
        type: "success"
  resolved:
    title: "Resolved"
    body: "Document steps and close ticket."
    options: []
---
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
  * `primary` - Blue/Indigo background (standard actions).
  * `success` - Green background (resolutions/clearing checks).
  * `danger` - Red background (escalations/error pathways).
  * `help` - Outlined styling with an external link indicator.

<div class="not-prose my-6 grid grid-cols-2 gap-4">
  <button class="w-full inline-flex items-center justify-center px-6 py-3 border border-transparent text-base font-semibold rounded-xl text-white bg-indigo-600 shadow-sm">primary</button>
  <button class="w-full inline-flex items-center justify-center px-6 py-3 border border-transparent text-base font-semibold rounded-xl text-white bg-emerald-600 shadow-sm">success</button>
  <button class="w-full inline-flex items-center justify-center px-6 py-3 border border-transparent text-base font-semibold rounded-xl text-white bg-rose-600 shadow-sm">danger</button>
  <button class="w-full inline-flex items-center justify-center px-6 py-3 border border-slate-300 text-base font-semibold rounded-xl text-slate-700 bg-white shadow-sm">help</button>
</div>

---

## Authoring Guide for New Flows

1. Create a new `.md` file in the `_flows/` directory (e.g., `_flows/database-cpu-alert.md`).
2. Add your YAML front matter (`---`) with `metadata` details at the top. Be sure to include a `type` string to group the flow appropriately on the homepage.
3. Map out your decision nodes under the `steps:` block. Sketch the flowchart beforehand to identify the step keys.
4. Ensure the entry point is named `start`.
5. Connect your nodes by linking options with their corresponding `target` ID.
6. Close your front matter with `---`.
7. Verify your changes. Since Jekyll runs in a development container, saving the file will immediately compile and display the new runbook on your dashboard.

</div>
