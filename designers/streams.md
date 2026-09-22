
Streams are the primary record type within Emissary.  They are the multi-purpose objects that are sculpted into all the rich, social applications you build with Emissary.

In their simplest form, each Stream is it's own web page, which is bound to a single [Template](/templates) to give it interactive [Actions](/actions).  For example, this page of documentation is a Stream, which uses the Article - Markdown template to define its data and behavior.

[Template designers](/creators) define all of the behaviors that each Template (and it's streams) can take.  [Site Owners](/admins) install these templates on their servers, and then [End Users](/people) interact with them according to their access permissions.

## Common Data

Every stream includes common data fields.  Whether they are used by a particular template or not, they are always there.

| Field     | Description                                                                             |
| --------- | --------------------------------------------------------------------------------------- |
| StreamID  | Hex value that uniquely identifies this Stream                                          |
| Token     | Unique value of (letters, numbers, dash, and underscore) used to generate URLs          |
| ParentID  | ID of this stream's parent.  Zeroes if this is a top-level stream                       |
| Rank      | Sort order to use when displaying this Stream                                           |
| StateID   | The current state of this stream, which drives the state machine and access permissions |
| Label     | The name, or title of this stream.  Displayed in auto-generated navigation              |
| Summary   | A medium-length description of this content                                             |
| Content   | HTML content that makes up the main body of this stream                                 |
| Tags      | An array of Tag objects that organize and categorize this stream                        |
| InReplyTo | If this stream is a "reply" to another object, that object's URL is stored here         |

<br>

These data values are accessed through the [Builder](/builders) that is bound to the page request.  They can be embedded in the resulting HTML using the standard syntax for [Go Templates](https://pkg.go.dev/html/template).

```html 
<tr><td>{{.Label}}</td></tr>
```

## Custom Data

In addition to common data fields, template designers can use the template's [schema](/schemas) to define any custom data necessary for their applications.  You can access custom data using values like `{{.Data "field-name"}}` in your HTML templates.

## Real-Time Updates with SSE

Emissary publishes updates to every stream in real-time using [Server Sent Events](https://en.wikipedia.org/wiki/Server-sent_events).

To listen for updates, subscribe to the SSE channel at `/<StreamID>/sse`.  The easiest way to accomplish this is using the [built-in HTMX] (/htmx) library, like this:

```
<div hx-get="/{{.StreamID}}" hx-trigger="sse:{{.StreamID}}"  hx-sse="connect:/{{.StreamID}}/sse">
```
