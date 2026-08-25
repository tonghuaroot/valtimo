# 13.43.0

Release date: 26-08-2026

---

## New Features

### Process bean selection

Expression fields in the BPMN modeler now offer a dropdown mode for selecting process beans and their methods. Pick from available services and methods instead of typing expressions manually.

### Activity markers

Visual indicators on BPMN elements show configuration at a glance: **P** for process link, **E** for execution listener, **T** for task listener. Spot configured activities instantly without opening the properties panel.

### Autofill tracking

Properties auto-filled by Valtimo are now marked in the properties panel. Dismiss the indicator after reviewing to keep your panel clean.

---

## Enhancements

### Smarter start event validation

Start events without forms no longer trigger warnings when the process isn't user-startable. Fewer false positives during process validation.

### Standardized validation error codes

Validation messages now use a consistent error code format, making it easier to identify and troubleshoot issues.

### Case definition key and version in case inspection

The metadata tab of the case inspection page now displays the case definition key and version. This makes it clear which version of a case definition a case belongs to.

### Building block call activities are now validated

The configuration of a building block call activity is checked when the process is saved and when the call activity starts. Mistakes that previously made a building block silently work on the wrong case data — such as a missing or wrong business key mapping — now block the save, and the process editor highlights the call activity with a message that explains how to fix it. See the [building block documentation](../../../configuration-guides/building-blocks/processes.md) for the call activity requirements.

### Clearer rules for building block input and output mappings

Values passed to a building block are stored in its document, and results are read back from it. Mappings that do not follow this are now rejected when the process is saved, with the offending activity highlighted in the process editor, instead of being silently ignored at runtime. How data flows in and out of a building block is described in the [building block documentation](../../../configuration-guides/building-blocks/processes.md).

### Better diagnostics for plugin actions

When a plugin action property resolves to no value, a debug log entry now names the property, the activity and the process definition, making it easier to trace why an action behaves as if a value was never provided.

---

## Bugfixes

| Area | Fix |
|------|-----|
| BPMN modeler | Orphaned invisible elements cleaned up on save |
| Draft environments | Default Spring profiles now correctly enable draft mode |
| Search fields | Date searches return results with correct date format |
| Cases | A case with building blocks can be deleted again |
| Document schemas | Recursive schema references no longer crash the server |
| Cases | A case that cannot be found no longer stops a process, an assignment or a note |
| Dashboard | Donut charts with many categories display the circle correctly |
| Process links | Links no longer leak into another case definition or building block |
| Notificaties API | Subscription registration no longer causes a restart loop on startup |
| Case export | Forms shown in a widget are included in the case export |
| Case notes | The options menu of a note is now correctly translated |
| Tasks | Tasks of cases that were already running before the upgrade to 13 can be opened again |
| Case widgets | Long texts wrap correctly, without overlapping other content |
| Widgets | The image widget no longer offers `task:` fields it cannot show |
