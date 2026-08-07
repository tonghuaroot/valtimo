# 13.41.0

{% hint style="info" %}
**Release date 12-08-2026**
{% endhint %}

## New Features

* **New feature title**

  New feature explanation.

## Enhancements

* **Building block call activities are now validated**

  The configuration of a building block call activity is checked when the building block link is saved and when the
  call activity starts. Mistakes that previously made a building block silently work on the wrong case data now fail
  with a message that explains how to fix them. See the
  [building block documentation](../../../features/building-blocks/README.md) for the call activity requirements.

* **Clearer rules for building block input and output mappings**

  Values passed to a building block are stored in its document, and results are read back from it. Mappings that do
  not follow this are now rejected with a clear message when the building block link is saved, instead of being
  silently ignored at runtime. How data flows in and out of a building block is described in the
  [building block documentation](../../../features/building-blocks/README.md).

* **Better diagnostics for plugin actions**

  When a plugin action property resolves to no value, a debug log entry now names the property, the activity and the
  process definition, making it easier to trace why an action behaves as if a value was never provided.

## Bugfixes

* **A divider widget without a title no longer shows a dash**

  A divider widget that is configured without a title now stays empty, both in the widget list on the
  widget management page and on the widget tab of a case. Previously a `-` was shown in both places as a
  placeholder for the missing title. In addition, saving a divider without a title on an IKO view no longer
  fails: the back end required a non-blank title for every widget, while a divider does not need one.

* **A divider widget can be duplicated again**

  Fixed an issue where duplicating a divider opened the duplication dialog with an empty, invalid key that 
  could not be edited, leaving the Duplicate button disabled. The dialog now pre-populates the divider key 
  with a unique default value and allows it to be edited before duplicating.
  
* **Start form of a building block now opens in the panel**

  Starting a building block from the 'Start' menu of a case did nothing when the start form of its
  main process is configured to be shown in a panel. The panel now opens right away. Previously it
  only appeared after first opening the start form of a regular process in the panel.
