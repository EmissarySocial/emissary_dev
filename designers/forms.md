
Emissary uses a [simplified version of JSON-Forms](https://github.com/benpate/form) to define HTML forms.  Forms describe the user interface, while [Schemas](/schema) map out the structure of the data.  The two work together to generate safe, validated data inputs that can be defined by app developers.

## Example JSON-Form

```json
{
	"form": {
		"type":"layout-vertical",
		"children": [
			{"type":"text", "path":"name", "label":"Name"},
			{"type":"select", "path":"email", "label":"Group"},
			{"type":"toggle", "path":"isActive", "label":"Active?"},
		]
	}
}
```

## Parameters

Every form element accepts the same set of parameters.

| Name        | Description                                                                                        |
| ----------- | -------------------------------------------------------------------------------------------------- |
| type        | The data type of the form element to display (detailed below)                                      |
| id          | ID of the element - needed by some widgets to identify themselves in the HTML DOM                  |
| path        | Dotted path to the field in the target object.  For instance `label` or `data.customValue`         |
| label       | Human-friendly label to display next to the field                                                  |
| description | Human-friendly description (or hint) often displayed below the field in gray text                  |
| children    | An array of additional elements used by layouts to display inside this widget.                     |
| readOnly    | A boolean value.  If true, then this field value is displayed in plain HTML and cannot be changed. |
| options     | Type-specific options (such as lookup providers) required by individual data types                 |

## Data Types

| Widget            | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| checkbox          | Group of checkboxes defined by the provided `Lookup Code`    |
| heading           | Static HTML heading based on the provided value              |
| hidden            | Includes an invisible "hidden" element in the resulting HTML |
| label             | Static HTML label based on the provided value                |
| layout-group      | Displays `children` elements in a grouped layout             |
| layout-horizontal | Displays `children` elements in a horizontal layout          |
| layout-tabs       | Displays `children` elements in a tabbed layout              |
| layout-vertical   | Displays `children` elements in a vertical layout            |
| multiselect       | Custom multiple selection widget (similar to checkbox)       |
| radio             | Group of radio buttons defined by the provided `Lookup Code` |
| select            | Single selection box defined by the provided \`Lookup Code   |
| text              | Single-line text box                                         |
| textarea          | Multi-line text box                                          |
| toggle            | Custom toggle widget (similar to checkbox)                   |


## Lookup Codes

Several content types require additional data to render -- such as checkboxes, radio buttons, and select boxes.  These lookup codes are defined in one of several places:

#### 1. Lookup Provider

Emissary includes a number of standard "lookup providers" that can be used to populate a widget.

| Provider               | Description                                                                                |
| ---------------------- | ------------------------------------------------------------------------------------------ |
| following-behaviors    | Lists the different kinds of messages that can be imported from a "Following" source.      |
| following-rule-actions | List the different ways that rules can be imported from a "Following" source.              |
| rule-actions           | Lists the different actions that can be taken when a rule is triggered on incoming content |
| rule-types             | Lists the different kinds of rules that can be defined in the system                       |
| folders                | Lists all folders present in the current user's inbox (Writable)                           |
| folder-icons           | Lists all of the pre-defined icons that can annotate an inbox folder                       |
| groups                 | Lists all of the User Groups present in the system (Writable)                              |
| sharing                | Lists the different options for simple sharing                                             |
| themes                 | Lists all of the themes defined in the system                                              |

#### 2. Element "Enum"

If the form element includes an enumeration in the form of: `"options":{"enum":["value 1", "value 2", "value 3"]}` then these values will be used to populate the widget.

#### 3. Schema "Enum"

If the schema definition of the data type includes an enumeration in the form of `enum:["value 1", "value 2", "value 3"]` then these values will be used to populate the widget

