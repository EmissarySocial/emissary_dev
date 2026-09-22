
Just like routes in a modern web application, every [Template](/templates) in Emissary includes several **action pipelines** that define the operations that can be taken on it.  These routes/actions apply to every Stream that uses this Template.  For instance, every template must have actions named: `create`, `view`, `edit`, and `delete`.

## Actions and Steps

Actions are comprised of one or more pre-built [steps](/steps) that are included with Emissary.  Emissary currently defines than 50 steps, with more being added as necessary.  For instance, the "view-html" step uses a pre-defined HTML template to [build content](/builders), and the "as-modal" step wraps content in a modal dialog box.  Used together, you can define custom pop-up dialogs with content customized to the stream that the user is currently viewing.

### Request Method Behaviors: GET and POST

Every step in a pipeline can perform different operations depending on the HTTP request method.  In general, `GET` requests will only display information without making changes, while `POST` requests will make changes, such as: updating the database, or sending data to external servers.

For example, for a `GET` request, the `delete` step displays a confirmation dialog that asks the user if they *really* want to delete an object.  When the user sends a `POST` to the same step, the object is deleted from the database.

While this is different from how many other programming environments work, it is very useful within Emissary, allowing you to write concise code like this:

``` json
{
	photo: {
		steps:[
			{do:"as-modal", steps:[
				{do:"view-html", file:"photo"}
				{do:"delete-attachments", all:true}
				{do:"upload-attachments", maximum:1}
				{do:"set-thumbnail", path:"imageId"}
				{do:"save", comment:"Profile photo updated by me"}
			]}
		{do:"reload-page"}
	]
}
```

The code above is for a photo upload widget.  When the user sends a GET request, it: 1) builds content from the "photo.html" template and 2) wraps that content in a modal dialog.  The other steps have no effect on this request.  Then, when the user submits the form in that HTML (as a POST request), this action: 1) removes any previous attachments, 2) uploads a maximum of 1 new attachment, 3) sets it as the thumbnail for this Stream, 4) saves the stream with the new thumbnail, and then sends a "reload page" event to the browser.

## Access Permissions

Action Pipelines work alongside Emissary's permission system.  Custom access rules are defined for every action, based on the user's role(s) and the object's state.  The available roles are defined in the stream template, then users can be assigned to various roles in groups by the owner of that stream.

```json
{
	"roles": {
		"viewer": {
			"label": "Viewer",
			"description": "Can view, but cannot make changes",
		},
		"editor": {
			"label":" Editor",
			"description": "Can make changes, but cannot publish it."
		},
		"publisher": {
			"label": "Publisher",
			"description": "Can publish this stream to the website."
		}
	}
}
```

### Assigning Users to Roles

To assign users to roles, make an action that uses the `set-simple-sharing` step, which implements a complete role editor.
```json
{
	// List of actions for this Template
	actions: {
		// Configuration for the "share" action.
		share: {
			// Only site owners are allowed to perform this "share" action.
			"roles: ["owner"]    
			steps: [
				// Open the simple sharing step in a modal dialog
				{do:"as-modal", steps: [
					
					// Use the "simple-sharing" assign users to the "viewer" role
					{do:"set-simple-sharing", roles: ["viewer"]}
					
					// After assigning roles, save the Stream to the database
					{do:"save", message:"Sharing updated by {{.Author}}"}
				]}
			]
		}
	}
}
```

### Magic Roles

Emissary adds "magic" roles to every request, depending on the user's authentication, profile, and relationship to the requested record.

| Role            | Description                                                                                                      |
| --------------- | ---------------------------------------------------------------------------------------------------------------- |
| `anonymous`     | Present for all requests, whether the user is signed in or not.                                                  |
| `authenticated` | Present if the authenticated user has signed in to this website.                                                 |
| `author`        | Present if the authenticated user was the author of the current stream.                                          |
| `self`          | Present if the authenticated user matches the requested inbox or outbox.                                         |
| `owner`         | Present if the authenticated user is a site owner.  Owners automatically have permission to perform all actions. |

### Enforcing Roles

Templates use roles to specify which users can perform which actions.  If a user's permissions match *any* of the provided roles, then the user is allowed to perform both the GET and POST versions of the action.

#### Roles Element

Use the "roles" to perform a simple match between the user and one or more roles.  If the User has been assigned one or more of the named roles, then they are allowed to perform this action.

```json
{
	"actions": {
		"view": {
			"roles": ["viewer", "editor", "publisher"],
			"steps": [ ... ]
		}
	}
}
```

#### StateRoles Element

For more sophisticated permission rules, you can also specify which roles are allowed *based on the current state of the stream*.  Using the "stateRoles" element, users will only be allowed if they have the required role and the stream is in one of the required states.

In this example below, "owners" and "editors" are always allowed to perform the "view" action on this stream, and "viewers" are only allowed if the stream is currently in the "published" state.

```json
{
	"actions": {
		"view": {
			// These roles are always allowed
			"roles": ["editor", "owner"]

			// The "viewer" role is allowed only when the stream is "published" state
			"stateRoles": {
				"published": ["viewer"]
			}
			steps:[ ... ]
		}

```

## Required Actions

Templates can define as many actions as required to perform their duties.  However, to function correctly, every Template must implement these required actions:

| Action | Description                                                                                                                                                                                  |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create | Called when a new object of any type is created. <br><br>GET: sets initial values and displays an "create" form (if required) <br> POST: saves the new object and forwards to the next page. |
| View   | Default view of the object's contents.  Is only called as a GET, never as POST.                                                                                                              |
| Edit   | Default page for editing an object's contents.  <br><br>GET: displays an edit page. <br>POST: updates the object's contents                                                                  |
| Delete | Delete widget. <br><br>GET: Displays a modal dialog to confirm the deletion. <br>POST: Deletes the object and performs necessary cleanup.                                                    |

