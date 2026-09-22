
### TL/DR; Emissary's user-facing UI works great as a programmatic API into the database, too.  You can load records as JSON-LD, or send update transactions by sending URL-Encoded form data.

<br>

Emissary apps are creating using [Templates](https://emissary.dev/templates), which define the structure, behaviors, and layouts of individual records, called [Streams](https://emissary.dev/streams).  Templates define the application states and [API Actions](https://emissary.dev/actions) that let you interact with Emissary programmatically, and Streams are the specific records you interact with.  Templates must define standard actions (like `create`, `view`, `edit`, and `delete`) and will usually define custom custom actions that are specific to individual templates.

## API Keys
This is under development.  For now, sign in to your Emissary database as a domain owner, then use your browser's developer tools to find the cookie named `Authorization`.  For now, this is your API key.

## GET Requests
Following the HTTP specifications, GET requests only retrieve information and never change the database.

With GET requests, most actions in Emissary will return HTML that is designed to be viewed in a browser, and not suitable for a programmatic API.

However, you can retrieve data for any [`Stream` builder](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Stream) or [`Outbox` builder](https://pkg.go.dev/github.com/EmissarySocial/emissary/builder#Outbox) record by making a GET request with the header `Accept: application/activity+json`.  

| Object | URL                          | Description                                                                                           |
| ------ | ---------------------------- | ----------------------------------------------------------------------------------------------------- |
| Stream | https://servername/token     | Returns JSON-LD that defines this ActivityPub document.  Defined by the Template used for this Stream |
| Outbox | https://servername/@username | Returns JSON-LD that defines the ActivityPub Actor                                                    |

## POST Requests
Following the HTTP specification, POST requests can add/update data in the database. Emissary deviates slightly from the HTTP specification by using POST requests to also delete records from the database as well.

Provided you have the proper authorization token, you can POST directly to any  route of an Emissary app to make changes.

### Routes
Specific API endpoints are defined by the Template being used.  Templates must always define `/create`, `/edit`, and `/delete` endpoints, which are good starting places for working with the Emissary API.

Other templates will define routes that are specific to that kind of data.  For instance, templates in a complex app may publish a route for adding child records underneath the parent.

### Parameters
The available fields for any transaction are defined in the Template.  For "edit" actions, you'll want to send fields in the form, as named by the `path` attribute, which allows transactions to refer to a highly nested data structures in an HTML form. See the example below for more specifics

### Form Actions
The most common kind of API endpoint is a custom form, which you'll define using an [`edit` step](https://emissary.dev/steps#63a8e0724f3c99d0e50d2372).  For GET requests this step returns an HTML form where users will add/update values.  For POST requests, it this step updates the object with those values.

**IMPORTANT!!** Edit forms will only touch the data that is defined in the form.  If you want to edit a specific property of a Stream in a particular API endpoint, this property MUST be defined in the edit form.  This is important for user roles and permissions, allowing Template designers to control which aspects of an object are editable by different groups of users.

### Other Actions

### Form Encoding
Because Emissary's APIs are used primarily as a web-based UI, transactions are encoded with either `application/x-www-form-urlencoded` or `multipart/form-data`, and NOT `application/json`.


## Example Code
For this example, we're going to edit a stream that uses the [photograph template](https://github.com/EmissarySocial/emissary/blob/main/_embed/templates/stream-photograph/template.hjson) from the Emissary default library.  Here's a snippet of the "edit" action:

### Sample Edit Action
```json
edit: {
	roles: ["owner", "editor"]
	steps: [
		{
			do: "as-modal"
			steps: 
			[
				{
					do: "edit"
					form: 
					{
						type: "layout-vertical"
						label: "Edit Photograph"
						children: 
						[
							{
								type: "text"
								label: "Title"
								path: "label"
							},
							{
								type: "textarea"
								label: "Summary"
								path: "summary"
							}
						]
					}
				}
				{do: "save", comment: "Updated by {{.Author}}"}
			]
		}
	]
}
```

This action will be executed when a web client makes either a GET or a POST request to: `https://<<SERVER_NAME_HERE>>/<<STREAM_TOKEN_HERE>>/edit`.  Let's read this to see what it does.

The `roles` attribute says that this action is only available to editors and owners.  The `owners` role is automatically given to users who are marked as domain owners.  The `editors` role is assigned to this stream specifically, using one of the many "permissions" actions.

If it is a **GET** request, Emissary will return HTML code that renders a modal dialog.  The dialog will contain an HTML form with two fields, named `label` and `summary`.  For our purposes, this isn't what we want.

If this is a **POST** request, Emissary will look at the request body, decoding it as `application/x-www-form-urlencoded`, then apply the parameters to the stream.  To protect the security of the system, Emissary will ONLY update the paths defined in this action.

### Sample Form Post
To update this photograph, we must send the following HTTP request:

```
POST https://<<SERVER_NAME_HERE>>/<<STREAM_TOKEN_HERE>>/edit HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Cookie: Authorization=<<API_KEY_HERE>>

label=Test&summary=Testy+McTestface
```
