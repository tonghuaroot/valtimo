# 13.40.0

{% hint style="info" %}
**Release date 05-08-2026**
{% endhint %}

## New Features

* **New feature title**

  New feature explanation.

## Enhancements

* **New enhancement title**

  New enhancement explanation.

## Bugfixes

* **List action menus no longer detach from their trigger in scrolled lists**

  The row action menu (⋮) of lists now always opens directly below its trigger, also when the list has many columns
  and a horizontal scroll bar. The menu pane is rendered at document level so surrounding layout (scroll containers,
  modals) can no longer displace or clip it, and when the trigger is scrolled out of view while the menu is open, the
  menu is hidden instead of floating detached over unrelated content.
