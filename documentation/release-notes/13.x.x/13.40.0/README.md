# 13.40.0

{% hint style="info" %}
**Release date 05-08-2026**
{% endhint %}

## New Features

* **New feature title**

  New feature explanation.

## Enhancements

* **Building block call activities are now validated**

  The business key configuration of a building block call activity is checked when the building block link is saved
  or updated, and again when the call activity starts. A missing, incorrect, or conflicting configuration now fails
  with a clear message that explains how to fix the BPMN. Previously, such a misconfiguration made the building block
  silently run against the wrong case data, with hard-to-trace symptoms such as forms not being prefilled or email
  attachments not being sent. The validation also detects configurations where a correct `camunda:` mapping is
  present but ignored by the engine because `operaton:` extension elements are used on the same call activity.
  Existing configurations that were silently broken will now report an error when the call activity starts, so they
  can be found and fixed.

* **Clearer rules for building block input and output mappings**

  Values passed to a building block are stored in its document, and results are read back from it. Input mappings
  must therefore target a building block field, and output mappings must read from one. Mappings that do not follow
  this are now rejected with an explanation when the building block link is saved, instead of being silently ignored
  at runtime.

* **Better diagnostics for plugin actions**

  When a plugin action property resolves to no value — for example an attachment list that references a field that
  does not exist in the current context — a warning is now logged that names the property, the activity, and the
  process definition. This makes it much easier to find out why a plugin action behaves as if a value was never
  provided.

## Bugfixes

* New bugfix.

## Documentation

* The [building block documentation](../../../features/building-blocks/README.md) now describes how data reaches a
  building block, how to pass files as email attachments by reference, and the call activity requirements.
