# 13.44.0

Release date: 02-09-2026

---

## New Features

### Visual form flow editor (beta)

Form flows can now be built in a visual editor instead of writing JSON by hand. When a form
flow is opened in case management or building block management, the editor opens on the
familiar **JSON editor** tab; the new visual editor is available on the **Editor (beta)**
tab. Both tabs work on the same definition, so you can switch between them at any time.

The visual editor shows the steps of the flow in a sidebar and the configuration of the
selected step next to it. Per step, the key, title and step type can be set, and the
type-specific configuration is offered as a choice: the **Form** dropdown lists the forms of
the surrounding case definition or building block, and the **Component ID** dropdown lists
the custom components registered by the implementation (the `custom-component` type is
unavailable when none are registered). Any step can be marked as the start step, and renaming
a step key automatically updates the start step and every transition that referenced it.

Transitions to next steps are configured per step, including their SpEL conditions and
evaluation order. Actions that run when a step opens, completes, or when the user navigates
back can be added from a menu that lists the registered form flow functions — such as
`valtimoFormFlow.completeTask` — with their parameters. Inline help explains how conditions
and actions work, and a help dialog documents exactly which data is available in
`additionalProperties`, based on what the application provides.

The editor validates the definition while editing (duplicate step keys, missing start step,
transitions to unknown steps, at most one default transition) and warns when leaving the page
with unsaved changes. See the
[form flow documentation](../../../configuration-guides/cases/form-flows.md#editing-in-the-visual-editor-beta)
for details.

---

## Enhancements

### New enhancement title

New enhancement explanation.

---

## Bugfixes

| Area | Fix |
|------|-----|
| Cases | The process selector on the Progress tab shows long process names in full instead of cutting them off |
| Process links | Selecting a different form flow definition on an existing form flow process link is now saved. Previously the change was silently ignored while display type and size changes were saved, which made it easy to miss. |
