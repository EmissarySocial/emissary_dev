
Themes defined the global behavior of a website, including the global "chrome" that makes up the header, footer, and potential sidebars of a website.  Themes should include a global stylesheet as a [resource bundle](/bundles) along with other common elements of the website.

## Creating and Distributing Themes

Each Theme is just a collection of files in a directory that Emissary knows to scan.  This can be a directory in the local filesystem, or a remote Git repository that is registered in the server configuration.  Emissary will recognize a directory as a theme if it contains a `theme.json` (or `theme.hjson`) definition file (as opposed to a `template.json` file or a `widget.json` file).  

## Theme File Format

The default theme bundled with Emissary is a good example of this structure. 

| Property       | Description                                                                    |
| -------------- | ------------------------------------------------------------------------------ |
| themeId        | Unique name used to identify this theme                                        |
| label          | Human-friendly label, displayed in theme lists                                 |
| description    | Human-friendly description, displayed in theme lists                           |
| rank           | integer sort order for ordering this theme in lists                            |
| extends        | Array of zero or more Unique IDs that this theme extends                       |
| bundles        | Describes [resource bundles](/bundles) that are packaged with this Template    |
| startupStreams | Defines initial streams (and content) to add when this Theme first is selected |
| startupGroups  | Defines initial user groups to add when this Theme first is selected.          |
| startupFolders | Defines initial folders to put into every new user's inbox.                    |
| defaultInbox   | Template to use when displaying user's inboxes. Defaults to "user-inbox"       |
| defaultOutbox  | Template to use when displaying user's outboxes. Defaults to "user-outbox"     |
| isVisible      | If TRUE, this theme is displayed in selection lists.                           |


<br>

Here is an example from Emissary's [default theme](https://github.com/EmissarySocial/emissary/tree/main/_embed/templates/theme-global) 

```json
{
	"themeId": "default",
	"extends":["global"],
	"isVisible":true,
	"category":"General",
	"label": "Minimus",
	"description": "Clean, top nav theme for general purpose websites.",
	"rank": 0,
	"bundles": {
		"stylesheet": {
			"content-type":"text/css"
		},
		"javascript": {
			"content-type":"text/javascript"
		},
		"hyperscript": {
			"content-type":"text/hyperscript"
		},
		"registration": {
			"content-type":"text/hyperscript"
		}
	},
	"startupStreams": [
		{templateId:"article-editorjs", token:"home", label:"Welcome!", rank:1},
		{templateId:"article-markdown", token:"about", label:"About Maximus", rank:2},
		{templateId:"article-markdown", token:"join-the-team", label:"Join the Team", rank:3}
	],
	"startupGroups":[
		{"label":"Publishers"},
		{"label":"Subscribers"}
	],
	"defaultFolders: [
		{"label":"Friends"},
		{"label":"Family"},
		{"label":"Colleagues"}
	]
}
```

## Required HTML Files

The following HTML templates MUST be included in order for your Theme to function.  

| Filename            | Description                                                            |
| ------------------- | ---------------------------------------------------------------------- |
| page.html           | Defines the outer borders (or "chrome") of the website.                |
| register.html       | Standalone page displayed when new users sign up for an account        |
| reset-code.html     | Standalone page displayed when users request a password reset          |
| reset-confirm.html  | Standalone page displayed when users confirm their password reset code |
| reset-password.html | Standalone page displayed when users enter their new password          |
| signin.html         | Standalone page displayed when users sign in to the website            |


## Extending Existing Themes

Many themes share common elements, for example: the register and signin workflows should be pretty consistent, regardless of the rest of your website chrome.  In this case, you can use the "extends" element to name one or more existing themes that you want to extend.  This applies to all settings, templates, bundles, and resources.

When a file or resource is requested, if it is contained in the current theme then that value will be used.  If not, Emissary searches for it in each of the themes named by the "extends" element in order.  The first result found will be returned.  This makes it straightforward for you to reuse elements you've already defined in other themes without having to maintain multiple copies of the same resource.

All of the default themes in Emissary use this mechanism to inherit from the [global theme](https://github.com/EmissarySocial/emissary/tree/main/_embed/templates/theme-global).  This is an easy way for you to jumpstart your own theme development, but it is not required.  You can also make your own theme from scratch, or inherit from your own base theme definitions.