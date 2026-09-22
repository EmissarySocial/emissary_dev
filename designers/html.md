
Most Emissary [Templates](/templates) will need to display HTML contents.  This is done using the `view-html` [step](/steps), which wraps the current object in a corresponding "builder" class, then passes this builder to the named [Go HTML/Template](https://pkg.go.dev/html/template).  Here is an example from a `template.json` file:

```json
{
	"actions": {
		"view": {
			"steps": [
				{"do":"view-html"}
			]
		}
	}
}
```

In this template, the corresponding `view.html` is a standard [Go HTML/Template](https://pkg.go.dev/html/template) that might look like the example below.  Notice the replacement tokens in `{{}}`.  These values are provided by the `Builder` class generated for this page, which contains all of the data accessors required to build a complete application in Emissary.  Builders are documented in the section below.

```html
<div class="page" hx-get="/{{.StreamID}}" hx-trigger="refreshPage from:window" hx-target="this" hx-swap="outerHTML" hx-push-url="false">

	<div id="menu-bar">
		<div class="left">
			<a hx-get="/{{.StreamID}}/edit">Edit List</a>
		</div>
		<div class="right">
			<a hx-get="/{{.StreamID}}/delete" class="text-red">Delete</a>
		</div>
	</div>

	<article>
		<h1>{{.Title}}</h1>
		<div>{{.Summary}}</div>
		<div>{{.ContentHTML}}</div>
	</article>
</div>
```


## Common Builders

Most custom [Templates](/templates) will work with `Streams`, the data type that contains most content in an Emissary application.  Streams are backed by the [Stream Builder](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Stream) class.

In addition to Streams, there are several other kinds of builders which are used in specialized parts of an Emissary site.

All builders inherit methods from the [Common Builder](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Common), which provides data accessors for site-wide data, and commonly used queries.

| Builder                                                                          | Description                                                                                                              |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| [Folder](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Model)    | Created by `with-folder` step. Provides data accessors for Folder types using the generic Model builder.                 |
| [Follower](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Model)  | Created by `with-follower` step.  Provides data accessors for "Follower" types using the generic Model builder.          |
| [Following](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Model) | Created by `with-following` step.  Provides data accessors for "Following" types using the generic Model builder.        |
| [Inbox](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Inbox)     | Only available to routes beginning with `/@me/inbox`. Provides data accessors for the current user's inbox.              |
| [Message](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Model)   | Created by `with-message` step.  Provides data accessors for "Message" types using the generic Model builder.            |
| [Outbox](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Outbox)   | Only available to routes beginning with `/@userId/...`. Provides data accessors for the named user's profile and outbox. |
| [Response](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Model)  | Created by `with-response` step. Provides data accessors for "Response" types using the generic Model builder.           |
| [Stream](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Stream)   | Created for all other routes.  Provides data accessors for the currently visible Stream                                  |

## Admin Builders

The domain administrator can modify to core system features through routes beginning with `/admin`.  These hard-coded routes use their own builders, which are locked to admin use only.

| Builder                                                                                      | Description                                                  |
| -------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| [Admin Domain](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Domain)         | Domain-specific accessors for `/admin/domains` route         |
| [Admin Group](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Group)           | Group-specific accessors for `/admin/groups` route           |
| [Admin Navigation](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Navigation) | Top-level navigation accessors for `/admin/navigation` route |
| [Admin Rule](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Rule)             | Rule-specific accessors for `/admin/rules` route             |
| [Admin User](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#User)             | User-specific accessors for `/admin/users` route             |
