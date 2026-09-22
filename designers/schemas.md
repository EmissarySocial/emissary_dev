
Emissary uses [a subset of JSON-Schema](https://github.com/benpate/rosetta/tree/main/schema) to define and validate the custom data that is stored in each [Theme](/themes), [Template](/templates), and [Widget](/widgets).  

Schemas validate all user-generated data two times. First: by the web UI to validate inputs before being sent, and second: by the server before saving any records to the database.  This helps guarantee that malicious content cannot be injected into your web app.

## Schema Highlights

Here is a sample snippet from a template schema: 

```json
{
	"schema": {
			"type":"object",
			"properties": {
					"token": {"type":"string", "required":true},
					"document": {"type":"object", "properties": {
							"label": {"type":"string"},
							"summary": {"type":"string"}
			}},
					"content":{"type":"object", "properties": {
							"type": {"type":"string"},
							"raw": {"type":"string"},
							"html": {"type":"string", "format":"html"}
					}}
			}
	},
}
```

## Data Types

### Any
This data is not validated. Any information can be included here.

`{"type":"any"}`

### Array

This data must be an array.  The items in each array are defined as a sub-schema, with optional properties **minlength** and **maxlength** that set the bounds of the array.

`{"type":"array", "items":{"type":"string"}, "minlength":0, "maxlength":10, "required":false}`

### Boolean

This data must be a TRUE or FALSE.  A **default** value can be set which will be used if no data is available.

`{"type":"boolean", "default":true, "required":true}`

### Integer

This data must be a whole number.  Optional properties **default**, **minvalue**, **maxvalue**, **multipleof**, and *required*\* limit the values that are accepted.

`{"type":"integer", "default":1, "minvalue":0, "maxvalue":42" "multipleof"2, "required":true}`

### Number

This data must be a number with or without a decimal place.  Internally, numbers are stored as a `float64`.  It includes the same options as `integer`: **default**,**minvalue**, **maxvalue**, **multipleof**, and *required*\*, but works with floats instead of ints.

`{"type":"number", "default":1, "minvalue":0.1, "maxvalue":9.9", "multipleof":0.1, "required":false}`

### Object

This data is stored as a Go map or struct, depending on the context.  You can define sub-schemas for each of the **properties** within each Object, along with a list of **required** values that must be present in order for the Object to validate.  All schemas in Emissary templates should have an Object at the top level.

`{"type":"object", "properties":{ ... }, "required": ... }`

### String

This data must be a string value, although an optional **format** parameter can be used to validate the specific data within it.  Optional properties **minlength**, **maxlength**, and **required** further validate the string. 

`{"type":"string". "format":"html"}`

## String Formats

Additional validators are included for guaranteeing that strings match a particular format.  Some formats accept additional attributes, for example: the "lower" and "upper" formats allow you to specify a minimum number of lowercase or uppercase values.  Multiple formats can be included in a single string schema by separating each one with a space.  For example, you could use the following schema to define a common password rule that requires that a string have have a minimum of 12 characters, and must include at least one uppercase letter, one lowercase character, and one number:

`{"type":"string", "format":"minlength=12 upper=1 lower=1 number=1"}` requiring that all passwords .

Here are all of the formats that are available to string validators

| Name       | Example                           | Description                                                                                                                                                                                                                                |
| ---------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| date       | `"format":"date"`                 | PENDING                                                                                                                                                                                                                                    |
| dateTime   | `"format":"dateTime"`             | PENDING                                                                                                                                                                                                                                    |
| email      | `"format":"email"`                | Must be a valid email address (validated with built-in [ParseAddress](https://pkg.go.dev/net/mail#ParseAddress)                                                                                                                            |
| hostname   | `"format":"hostname"`             | PENDING                                                                                                                                                                                                                                    |
| html       | `"format":"html"`                 | Valid HTML. values are filtered using the [Blue Monday](https://github.com/microcosm-cc/bluemonday) [UGC Policy](https://pkg.go.dev/github.com/microcosm-cc/bluemonday?utm_source=godoc#UGCPolicy) to remove potentially dangerous content |
| ipv4       | `"format":"ipv4"`                 | PENDING                                                                                                                                                                                                                                    |
| ipv6       | `"format":"ipv6"`                 | PENDING                                                                                                                                                                                                                                    |
| in         | `"format":"in=one,two,three"`     | Must be in the list of options provided                                                                                                                                                                                                    |
| lower      | `"format":"lower=6"`              | Must contain a minimum number of lowercase letters                                                                                                                                                                                         |
| no-html    | `"format":"no-html"`              | Plain text without HTML tags. If no format is defined, this is the default.                                                                                                                                                                |
| notin      | `"format":"notin=excluded,words"` | Must NOT contain any of the list of excluded words                                                                                                                                                                                         |
| number     | `"format":"number=3"`             | Must contain a minimum number of digits                                                                                                                                                                                                    |
| objectId   | `"format":"objectId"`             | Must be a 24 character hexadecimal value                                                                                                                                                                                                   |
| regex      | `"format":"regex=abc123"`         | Must conform to the specified [Go Regexp](https://pkg.go.dev/regexp)                                                                                                                                                                       |
| time       | `"format":"time"`                 | PENDING                                                                                                                                                                                                                                    |
| token      | `"format":"token"`                | Can only contain letters, digits, dashes, and underscores (case insensitive)                                                                                                                                                               |
| unsafe-any | `"format":"unsafe-any"`           | Does not validate the string at all.  MUST NOT BE USED FOR USER CONTENT                                                                                                                                                                    |
| upper      | `"format":"upper=6"`              | Must contain a minimum number of uppercase characters                                                                                                                                                                                      |
| url        | `"format":"url"`                  | PENDING                                                                                                                                                                                                                                    |
