---
sidebar_position: 1
---

# Create a Form

## Kwesforms Setup

- During development, use `test` mode and configure your local environment file.
- The form `id` from the form configuration is what sets the form action. ([Create a form with Kwesforms](https://kwes.io/docs/v2/getting-started))
- `No-Validate`: Kwesforms has built-in field validation, and this disables native validation.

## Dynamic Fields

- Hidden fields can be dynamically set by using the `data-presets` attribute.
- Form configuration (JSON Endpoint) is defined using the `data-location` attribute.
- Supported field types:
  - `text`
  - `textArea`
  - `select` - expanded ability to include `multiple` and `size` attributes for multi-select.
  - `checkbox`
  - `checkboxGroup`
  - `password`
  - `radio`
  - `email`
  - `hidden`
  - `tel` - phone-specific input with `pattern` capabilities.
  - `range` - simple generic.
  - `number` - simple generic.
  - `date` - simple generic.
  - `time` - simple generic with basic validation based on min/max values.
  - `datetime-local` - suitable for date+time scheduling and allows for `min` / `max` dateLocale strings.
  - `datepicker` - renders a styled calendar when `kwesforms` is initialized.
  - `file` (requires Starter Plan for file uploads).
  - `cc-number` (TBD: Starter Plan: Credit card fields).

**HTML Example:**
```html
<div
  id="app"
  data-form-id="L6C4A1tTGJgxfefI6vtX"
  data-location="/form01.json"
  data-presets='[{"name": "Tester"},{"lname": "Jim"}]'
></div>
```

:::tip
Please note that the `data-form-id` and `data-location` attributes are required. You can add multiple `div` instances to embed multiple forms on the page.
:::

## JSON Configuration

Kwesforms *Starter Plan* is needed for customized Success & Error Messaging and additional features.
You can start out with the free plan and use that entirely until you need more features / submissions.

*You will replace the `id` with your own Kwesforms Form ID.*

- Basic Example:

```json
{
  "id": "SWXrdEPodyOKj9vXYmwU",
  "title": "Welcome!",
  "subTitle": "Please fill out the form below:",
  "disclaimer": "When you agree to these terms, you agree to allow us to contact you via email.",

  "submission": {
    "button": "Submit",
    "success": "Your form has been submitted successfully.",
    "error": "Oops! Your form has errors. Please check the form and try again."
  },

  "fields": [
    {
      "type": "text",
      "name": "first_name",
      "rules": "required",
      "label": "First Name"
    },
    {
      "type": "select",
      "name": "city",
      "rules": "required",
      "label": "Your City",
      "defaultOption": { "Select One": "" },
      "options": { "Buckeye": "Buckeye", "Avondale": "Avondale" }
    },
    {
      "type": "checkbox",
      "name": "terms",
      "rules": "accepted",
      "label": "I agree to the terms and conditions."
    }
  ]
}
```

## Field Groups

To use responsive columns, wrap the fields with a `group` array and specify a `size`. These can be interchangeably used with standard `field` definitions.

Replace `{column-size}` with the column number you want to use (see the [Bulma 12-column grid system](https://bulma.io/documentation/columns/sizes/#12-columns-system)).

**Example:**
```json
{
  ...
  "fields": [
    "group": [
        {
          "size": {column-size},
          "field": {...}
        },
        ...
    ]
  ]
}
```

:::tip
 Try to include your form configurations (JSON) files locally and serve them from the same domain (see `dist/public paths`).
:::

## TypeScript Types (for Reference)

These TypeScript types can be used as a helpful reference for creating dynamic forms in Kwesforms:

```typescript
interface Field {
  type: 'select' | 'text' | 'hidden' | 'checkbox' | 'checkboxGroup' | 'radio' | 'email' | 'textarea' | 'password' | 'date' | 'datepicker' | 'file' | 'datetime-local' | 'time' | 'tel' | 'range' | 'number';
  name: string;
  label: string;
  placeholder?: string;
  help?: string;
  rules?: string;
  options?: object;
  defaultOption?: object;
}

interface Group {
  group: { field: Field; size?: number }[];
}

interface FormData {
  id: string;
  title?: string;
  subTitle?: string;
  fields: (Field | Group)[];
  disclaimer?: string;
  submission?: { button?: string, success?: string, error?: string };
}
```

:::tip
Use these data types with openAi to auto-generate forms that meet your needs. 🤖
:::
