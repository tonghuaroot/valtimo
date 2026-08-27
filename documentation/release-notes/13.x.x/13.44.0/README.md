# 13.44.0

Release date: 02-09-2026

---

## New Features

### Visual form flow editor (beta)

Form flows can now be built visually instead of by hand-writing JSON. A new **Editor (beta)**
tab sits beside the existing **JSON editor** tab — both work on the same definition, so you can
switch at any time. The visual editor lists the flow's steps in a sidebar and, per step, lets
you set the key, title, type, start step, transitions (with their SpEL conditions and order),
and the actions that run on open, complete, or back. It validates the definition as you edit
and warns about unsaved changes. See the
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
| Plugins | Required fields such as the authentication configuration have to be filled in before a plugin configuration can be saved |
| Process links | Changing the form flow definition on an existing form flow process link is now saved (previously the change was silently ignored). |
