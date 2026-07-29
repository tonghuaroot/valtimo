# 13.39.0

{% hint style="info" %}
**Release date 29-07-2026**
{% endhint %}

## New Features

* **New feature title**

  New feature explanation.

## Enhancements

* **Better logging for Notificatie API subscriptions**

  Creating, updating, and deleting a Notificatie API subscription ("abonnement") is now logged.
  Successful changes are logged at `INFO` level and failures at `WARN`/`ERROR` level including the
  reason, making it easier to trace subscription problems. When subscription registration is turned
  off (`valtimo.zgw.register-abonnementen=false`), this is now logged instead of happening silently.

## Bugfixes

* **Case list export now enforces the export permission**

  The case list CSV export endpoint (`POST /api/v1/case/{caseDefinitionKey}/export`) now checks the `export` permission
  before producing a file. A user without an applicable `export` permission for the case definition now receives a
  403 (Forbidden) response instead of an empty CSV.

* **Tooltips no longer stay on screen when the hovered element is removed**

  A tooltip shown for an element that was removed or re-rendered while hovered (for example on pages that
  refresh their data periodically, such as the OpenSearch reindex overview) stayed visible on screen
  permanently. The tooltip is now cleaned up together with its element.

* **List rows keep their state during periodic data refreshes**

  Lists that refresh their data periodically re-created their rows on every refresh, causing expanded rows,
  tooltips and hover state to flicker or reset. Rows with a stable identity (such as lists using an expanded
  row key) are now updated in place, so the view stays stable while the data refreshes.

* **Consistent Dutch terminology for cases**

  Several Dutch interface texts used *zaak* where the standard term *dossier* applies, among others in the
  OpenSearch and generic case list feature toggles, case statuses and tags, retention settings, the metroline
  widget and access control. These texts now consistently use *dossier*. Dutch ZGW-related terms (such as
  *zaaktype* and *zaaknummer*) are unchanged.

* **Sorting on columns in the case list**
  
  When sorting the columns, the default sorting would not be respected in some cases.
  Additionally, when OpenSearch was used, sorting did not work. These issues have now been resolved.

* **Notificatie API subscriptions are now reliably (re)registered when plugin configuration changes**

  When a plugin that uses the Notificatie API (such as the Verzoek plugin) is added, changed, or
  removed, its subscription is now updated reliably. Previously the remote subscription could end up
  out of sync with the saved configuration, and failures went unnoticed.

* **Task permissions are re-evaluated after changing a task assignment**

  Permission checks in the task detail dialog were cached, so after assigning a task to another user the
  "Assign user" control stayed active even when the user no longer had the `assign` permission, and a second
  attempt was only rejected by the backend. Permissions for the task are now re-evaluated after every
  assignment change (including changes made by other users): the assign control is deactivated when the
  `assign` permission is lost, and the dialog closes automatically when the `view` permission is lost.
  Re-evaluating permissions this way no longer surfaces a spurious "access denied" error notification
  for the expected `403`/`404` responses (on the task re-fetch when the `view` permission is lost, or on
  the candidate user/team lookup when the `assign` permission is lost). For custom components,
  `PermissionService` now offers a public `invalidateResource(resource, identifier?)` method to clear
  cached permission results.

* **No spurious error notification when document types cannot be viewed in a task form**

  Opening a task whose form contains a document upload field no longer shows an "access denied" error
  notification when the user is allowed to open the task but not to view the case's document types. The
  document type lookup now treats a `403` as an expected outcome and falls back to an empty list.

* **Closing a dialog with the Esc key now works reliably**

  Pressing Esc now reliably closes the open dialog, even when you first clicked somewhere inside it
  that is not a button or input field. Previously the dialog could ignore Esc and had to be closed by
  refreshing the page.

## Security

* Addressed the reported security alerts. The `sigstore` dependency was updated to a fixed version. The remaining
  Angular alerts (hydration DOM clobbering, `HttpTransferCache` cache-key handling, and `formatDate` denial of
  service) were reviewed as non-exploitable in Valtimo's browser-only SPA and remain tracked for the next major
  Angular upgrade.
