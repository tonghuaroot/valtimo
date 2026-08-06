# Form flows

Form flow allows users to create a sequence of forms, similar to a flow chart or form wizard. A form flow definition contains the configuration for each step, what the subsequent steps are, and the conditions under which they can be reached. A form flow can be linked to a task, which consequently allows for more forms for the same task. Form flow supports forwards and backwards movement between steps without completing the task, reducing complexity in a BPMN model.

Form flow offers a way to configure a flow between different Form IO forms inside the same user task.

## Creating a form flow definition

A form flow definition is a JSON document that defines steps and which steps can be reached under what conditions when the current step is completed. Each step can have zero or more actions (e.g. retrieve external data) that trigger when the task is opened or completed. Depending on the type of step, the step can be handled in a different way. The way a form is handled can differ from a payment, for example.

For information on how to link a form flow definition to a task, see the [form flow process link documentation](../process/process-link.md#creating-a-form-flow-process-link).

{% tabs %}
{% tab title="Via UI" %}
* Go to the `Admin` menu.
* Go to the `Cases` menu and select the case to configure form flows for.
* Select the `Form Flows` tab.

Form flows can be added to draft case definitions via the **Create new form flow** button. A modal will be shown where the form flow key can be set and the form flow can be created.

After creating a form flow, it opens in the form flow editor. The editor has two tabs that work on the same definition: a visual **Editor** and a **JSON editor**.

**The visual editor**

<figure><img src="images/form-flow-editor.png" alt=""><figcaption></figcaption></figure>

The left panel lists the steps of the flow; **Add step** adds a new one. Selecting a step shows its configuration on the right:

* **Step details.** The key identifies the step; renaming it automatically updates the start step and every transition that references it. The optional title is shown in the [breadcrumb trail](form-flow.md#bread-crumbs). The type determines what the step shows: for a `form` step, the **Form** dropdown lists the forms of this case definition; for a `custom-component` step, the **Component ID** dropdown lists the custom components registered by the implementation. See the [step types section](form-flow.md#step-types) for more information.
* **Start step.** The step where the form flow begins carries a *Start step* tag. Any other step can be made the start step with the **Make start step** button.
* **Navigation.** Transitions define where the user can go after completing the step. Each transition points to another step and can have a SpEL condition. Transitions are evaluated from top to bottom — the first one whose condition holds is taken, and a transition without a condition is the default. The order can be changed with the arrow buttons.
* **Actions.** Expressions that run when the step opens, when it is completed, or when the user navigates back. The **Add action** menu lists the registered form flow functions with their parameters, next to the option to write a blank expression. See the [expressions section](form-flow.md#expressions) for more information.

<figure><img src="images/form-flow-editor-actions.png" alt=""><figcaption></figcaption></figure>

The **How do expressions work?** button opens a help dialog explaining conditions and actions, including exactly which data is available in `additionalProperties` for this application.

<figure><img src="images/form-flow-editor-help.png" alt=""><figcaption></figcaption></figure>

The editor validates the definition while editing — duplicate step keys, a missing start step, transitions to unknown steps, and multiple default transitions are reported — and warns when leaving the page with unsaved changes.

**The JSON editor**

The **JSON editor** tab shows the definition as JSON with schema-based validation and autocompletion. This is useful for copying definitions between environments or for editing properties in bulk. The JSON format is described in the **Via IDE** tab.
{% endtab %}

{% tab title="Via IDE" %}
To create form flow definition, the following steps are necessary:

* Create a form flow `.form-flow.json` file under the following path: `*/resources/config/case/{case-definition-key}/{version-tag}/form-flow/`.
* Add each individual step to the form flow definition, e.g.:

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" %}
```json
{
    "startStep": "personalDetailsStep",
    "steps": [
        {
            "key": "personalDetailsStep"
        },
        {
            "key": "loanApprovedStep"
        },
        {
            "key": "loanDeniedStep"
        },
        {
            "key": "summaryStep"
        }
    ]
}
```
{% endcode %}

* Add the next steps for each of the individual steps. This can be done through the `nextStep` property, which supports a single step, or `nextSteps`, which supports multiple next steps. Each step should correspond to the key of another step defined in this form flow.

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" %}
```json
{
    "startStep": "personalDetailsStep",
    "steps": [
        {
            "key": "personalDetailsStep",
            "nextSteps": [
                {
                    "step": "loanApprovedStep"
                },
                {
                    "step": "loanDeniedStep"
                }
           ]
        },
        {
            "key": "loanApprovedStep",
            "nextStep": "summaryStep"
        },
        {
            "key": "loanDeniedStep",
            "nextStep": "summaryStep"
        },
        {
            "key": "summaryStep"
        }
    ]
}
```
{% endcode %}

* The `personalDetailsStep` is now followed by two other steps. This is only allowed when at least one of the two steps is conditional. The order of next step matters. The first `nextStep` with `condition` that is evaluated to `true` will be the next step. When all the conditions are evaluated to `false` the next step will be the default step; which is the step without condition. If no next step is found, the form flow will end. The expression inside the condition is further explained in the [step types section](form-flow.md#step-types).

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" overflow="wrap" %}
```json
{
    "startStep": "personalDetailsStep",
    "steps": [
        {
            "key": "personalDetailsStep",
            "nextSteps": [
                {
                    "step": "loanApprovedStep",
                    "condition": "${step.submissionData.personalDetails.age >= 21}"
                },
                {
                    "step": "loanDeniedStep"
                }
           ]
        },
        {
            "key": "loanApprovedStep",
            "nextStep": "summaryStep"
        },
        {
            "key": "loanDeniedStep",
            "nextStep": "summaryStep"
        },
        {
            "key": "summaryStep"
        }
    ]
}
```
{% endcode %}

*   Configure the step type. Currently, the only step type supported is `form`, which requires a `definition` property to be set. This refers to the key of the form. For more information on step types, see the [step types section](form-flow.md#step-types).

    Which properties are required to be set depends on the step type.

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" overflow="wrap" %}
```json
{
    "startStep": "personalDetailsStep",
    "steps": [
        {
            "key": "personalDetailsStep",
            "type": {
                "name": "form",
                "properties": {
                    "definition": "personal-details-form"
                }
            },
            "nextSteps": [
                {
                    "step": "loanApprovedStep",
                    "condition": "${step.submissionData.personalDetails.age >= 21}"
                },
                {
                    "step": "loanDeniedStep"
                }
           ]
        },
        {
            "key": "loanApprovedStep",
            "type": {
                "name": "form",
                "properties":  {
                    "definition": "loan-approved-form"
                }
            },
            "nextStep": "summaryStep"
        },
        {
            "key": "loanDeniedStep",
            "type": {
                "name": "form",
                "properties":  {
                    "definition": "loan-denied-form"
                }
            },
            "nextStep": "summaryStep"
        },
        {
            "key": "summaryStep",
            "type": {
                "name": "form",
                "properties":  {
                    "definition": "summary-form"
                }
            }
        }
    ]
}
```
{% endcode %}

* Add triggers to steps where necessary (e.g. to store data externally). See below for more information.
{% endtab %}
{% endtabs %}

## Creating a Form.io form

{% tabs %}
{% tab title="Via UI" %}
**Completing a step**

To complete a step in the form flow, the button should be configured with:

* Action: `Submit`&#x20;

For example:

<figure><img src="https://valtimo.gitbook.io/valtimo-documentation/~gitbook/image?url=https%3A%2F%2F181910120-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FG21VIGcgPc1cyGRZY5CR%252Fuploads%252Fgit-blob-bfa70085bbb661044f4e138ee27566e876e5b6b1%252Fnext-button.png%3Falt%3Dmedia&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=23f9dfc0&#x26;sv=2" alt=""><figcaption></figcaption></figure>

**Going back to the previous form**

A button can be configured to go back to the previous form of the form flow. This 'Back' button should be configured with:

* Action: `Event`&#x20;
* Button event: `back`&#x20;

For example:

<figure><img src="https://valtimo.gitbook.io/valtimo-documentation/~gitbook/image?url=https%3A%2F%2F181910120-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FG21VIGcgPc1cyGRZY5CR%252Fuploads%252Fgit-blob-07be3b1bff180288611fa7df840ae1f626b9fc24%252Fback-button.png%3Falt%3Dmedia&#x26;width=300&#x26;dpr=4&#x26;quality=100&#x26;sign=94507b12&#x26;sv=2" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

## Step types

The following step types are supported by form flow:

### Form

The `form` step type is used to associate a step with a specific form. The following properties are supported:

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" %}
```json
    ...
      "type": {
        "name": "form",
        "properties": {
          "definition": "String" //The ID of the form
        }
      }
    ...
```
{% endcode %}

### Custom component

The `custom-component` step type is used to associate a step with a custom-made front-end component. The following properties are supported:

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" %}
```json
    ...
      "type": {
        "name": "custom-component",
        "properties": {
          "componentId": "String" //The key of the component in the front-end
        }
      }
    ...
```
{% endcode %}

More information on custom form flow components can be found in the [custom component documentation](forms/forms/create-custom-component.md).

## Expressions

Form flow supports Spring Expression Language (SpEL) expressions to allow for more complex actions when a step is opened or completed. Expressions can be recognized by the surrounding `${ }` characters. The following additional properties are supported for steps:

* `onBack`. Triggers the expressions when navigating to the previous step, e.g. to remove data from a document.
* `onOpen`. Triggers the expressions when the step is opened, e.g. to retrieve external data.
* `onComplete`. Triggers the expressions when the step is complete, e.g. to store the results in a document.

Each of these properties supports more than one expression, e.g. when a step is opened, external data from more than one source is retrieved. These expressions are evaluated in order.

Valtimo provides access to certain variables in the SpEL context, e.g. what the current step is. Which properties are available can be found in the [available SpEL context properties section](forms/forms/whitelist-spring-bean.md#available-properties-in-spel-context).

### Examples

The step condition below will only go to the step `loanApprovedStep` when a user has entered an age that is above 21:

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" %}
```json
{
    "step": "loanApprovedStep",
    "condition": "${step.submissionData.personalDetails.age >= 21}"
}
```
{% endcode %}

The `onOpen` expression below will call an external `@FormFlowBean` called `someService` to retrieve some data. The `additionalProperties` is a [form flow parameter](forms/forms/whitelist-spring-bean.md#available-properties-in-spel-context):

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" %}
```json
{
  "onOpen": [
    "${someService.retrieveData(additionalProperties)}"
  ]
}
```
{% endcode %}

The `onComplete` expressions below will change the data submitted by the user. It adds a `fullName` and removes the `firstName` and `lastName`:

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" overflow="wrap" %}
```json
{
  "onComplete": [
    "${step.submissionData.person.fullName = step.submissionData.firstName + ' ' + step.submissionData.lastName}",
    "${step.submissionData.firstName = null}",
    "${step.submissionData.lastName = null}"
  ]
}
```
{% endcode %}

The `onOpen` expressions delete any existing submission data of the step, before opening the form flow:

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" %}
```json
{
  "onOpen": [
    "${step.submissionData = null}"
  ]
}
```
{% endcode %}

An example of a complete form flow:

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" overflow="wrap" %}
```json
{
    "startStep": "personalDetailsStep",
    "steps": [
        {
            "key": "personalDetailsStep",
            "type": {
                "name": "form",
                "properties": {
                    "definition": "personal-details-form"
                }
            },
            "onOpen": ["${someService.retrieveData(additionalProperties)}"],
            "nextSteps": [
                {
                    "step": "loanApprovedStep",
                    "condition": "${step.submissionData.personalDetails.age >= 21}"
                },
                {
                    "step": "loanDeniedStep"
                }
            ]
        },
        {
            "key": "loanApprovedStep",
            "type": {
                "name": "form",
                "properties": {
                    "definition": "loan-approved-form"
                }
            },
            "onBack": ["${someService.removeData(additionalProperties)}"],
            "nextStep": "summaryStep"
        },
        {
            "key": "loanDeniedStep",
            "type": {
                "name": "form",
                "properties": {
                    "definition": "loan-denied-form"
                }
            },
            "nextStep": "summaryStep"
        },
        {
            "key": "summaryStep",
            "onComplete": ["${valtimoFormFlow.completeTask(additionalProperties, step.submissionData)}"],
            "type": {
                "name": "form",
                "properties": {
                    "definition": "summary-form"
                }
            }
        }
    ]
}
```
{% endcode %}

By default, SpEL allows access to every Spring bean from inside expressions. For security reasons, this has been changed to a whitelist instead. More information on how to whitelist Spring beans is available in the [Spring bean whitelist documentation](forms/forms/whitelist-spring-bean.md) and more information on SpEL can be found in the [Spring Expression Language reference](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#expressions).

## Bread crumbs

If the [feature toggle](../../running-valtimo/application-configuration/feature-toggles.md) `enableFormFlowBreadCrumbs` is enabled, users will see a new breadcrumb trail on top of every form flow. This allows a user to quickly navigate between the different steps of the form flow.

Every form flow step has an optional field called `title` which is shown in the example below. This field is used to display the title of the step inside the breadcrumb trail. If the title field is left empty, the breadcrumb trail will try to fill the title field by looking for existing translations inside the `en.json` or the `nl.json` files, that can be found in the frontend code.

The breadcrumb trail uses a simple way to predict which breadcrumbs to display in all future steps. The breadcrumb trail takes the first step inside the `nextSteps` field to determine all future steps. In the example below, the breadcrumb trail would be: `1. Personal details -> 2. Loan approved` because the `loanApprovedStep` is the first step in the `nextSteps` field.

{% code title="*/resources/config/case/loan/1-0-0/form-flow/example.form-flow.json" %}
```json
{
  "startStep": "personalDetailsStep",
  "steps": [
    {
      "key": "personalDetailsStep",
      "title": "1. Personal details",
      "type": {
        "name": "form",
        "properties": {
          "definition": "personal-details-form"
        }
      },
      "nextSteps": [
        {
          "step": "loanApprovedStep",
          "condition": "${step.submissionData.personalDetails.age >= 21}"
        },
        {
          "step": "loanDeniedStep"
        }
      ]
    }
  ]
}
```
{% endcode %}
