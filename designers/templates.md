
Emissary's templates determine much more than the way a particular site looks.  Templates can define their own [data schemas](/schemas), [workflow rules](/workflow), and other site behaviors.  Each one is a self-contained unit that can be loaded from your server's filesystem or downloaded from a networked Git repository.  This lets system administrators set the ground-rules for how their servers are to be used, while still giving website owners control over their individual domains.

The Emissary distribution includes a number of default templates to get started, although system administrators can use the [Setup Console](/setup-console) to remove these and add a set of custom templates instead.

## Creating and Distributing Templates

Each Template is just a collection of files in a directory that Emissary knows to scan.  This can be a directory in the local filesystem, or a remote Git repository that is registered in the server configuration.  Emissary will recognize a directory as a template if it contains a `template.json` (or `template.hjson`) definition file (as opposed to a `theme.json` file or a `widget.json` file).  

### Template File Format

| Property        | Description                                                                                                          |
| --------------- | -------------------------------------------------------------------------------------------------------------------- |
| templateId      | Unique name used to identify this template                                                                           |
| model           | Name of one of the [renderer models](/renderers) to use when rendering.  Almost always "stream"                      |
| extends         | Array of Template IDs that this Template extends (see below)                                                         |
| templateRole    | The role that this template takes in the system.  Used to determine where new streams can be placed in the hierarchy |
| socialRole      | ActivityPub object type to use when publishing streams to ActivityPub (e.g. Note, Article, Image, etc)               |
| containedBy     | Array of zero or more **template roles** that can contain this template.                                             |
| label           | Human-friendly label, displayed in template lists                                                                    |
| description     | Human-friendly description, displayed in template lists                                                              |
| icon            | Icon name, displayed in template lists                                                                               |
| widgetLocations | Array of region names where [widgets](/widgets) can be embedded. TOP, LEFT, BOTTOM, RIGHT are recommended.           |
| bundles         | Describes [resource bundles](/bundles) that are packaged with this Template                                          |
| schema          | Describes the [data schema](/schemas) of all custom data that this Template uses                                     |
| states          | Defines the **workflow states** that this Template uses (see below)                                                  |
| accessRoles     | Defines the **user roles** that this Template uses (see below)                                                       |
| actions         | Defines the **actions** that can be performed on streams that use this Template (see below)                          |

### Example JSON

Here is an example `template.json` file that includes some of these options:

```json
{
	"templateId": "photo",
	"socialRole":"Image",
	"model":"stream",
	"containedBy": ["photo-album"],
	"label": "Photograph",
	"icon": "picture",
	"schema": {
		"type": "object",
		"properties": {
			"document": {"type":"object", "properties": {
				"label": {"type": "string"},
				"imageUrl": {"type": "string"}
			}}
		}
	},
	"states": {
		"default": {
			"label": "Default State",
			"description": "Photos only have one state"
		}
	},
	"accessRoles": {
		"editor": {
			"label": "Editor",
			"description": "Can make changes to this item."
		},
		"readonly": {
			"label": "Read Only Access",
			"description": "Can view this photo, but cannot make changes"
		}
	},
	"actions": {
		"view": {
			"step": "view-html",
		},
		"edit": {
			"roles": ["editor"],
			"steps":[{
				"step":"as-modal",
				"steps": [{
					"step":"edit",
					"form": {
						"type":"layout-vertical",
						"label":"Edit Folder",
						"children": [
							{"type":"text", "label":"Label", "path":"document.label"},
							{"type":"textarea", "label":"Summary", "path":"document.summary"},
							{"type":"select", "label":"Format", "path":"data.format"},
							{"type":"select", "label":"Show Images", "path":"data.showImages"}
						]
					}
				}, 
			{"step":"save", "comment":"Updated Folder"}
			]}]
		},
		"delete": {
			"roles": ["author"],
			"steps": [
				{"step":"delete", "title": "Delete this Photograph?"},
				{"step": "forward-to", "url":"/{{.ParentID}}"}
			]
		}
	}
}
```

## Actions and Steps

Every Template defines a series of [actions](/actopms) that can be performed on the [Streams](/streams) that use that Template.  These actions might be as simple as "Create", "View", "Update", and "Delete" -- or they might define a sophisticated publishing and approval workflow that spans a number of individual users and roles.

There are three actions defined in the example `template.json` file above: "view", "edit", and "delete".  These actions can be as simple as setting some initial data, or loading an HTML template to display to the user, or they can define a complex behavior like the edit action.  

Each action is composed from a library of interchangeable [Steps](/steps) that provide pre-built functionality to your template design.  Steps each have their own rules and parameters.  Some are very simple and only do one thing.  Others can contain a large number of options -- including other steps -- to facilitate complex branching and workflow rules.  All steps are pre-compiled when the template is loaded in order to maximize server performance.

## HTML Content

In the action list above, the "view" action displays an actual web page.  This HTML content is packaged into the template as a [Standard Go Template](https://pkg.go.dev/html/template).  Like [Themes](/themes) and [Widgets](/widgets), These content templates are stored as separate files in the template directory, that must all be named with the `*.html` suffix.

## State Machines

Every stream in Emissary is also a mini state machine.  Each template can define the various states that its streams support.  For example, the built-in article templates include "published" and "unpublished" states, while the built-in comment templates use "visible", "hidden", and "awaiting-moderation".  Here's an example snippet from the built-in Article template:

```json
{
    "states":{
        "unpublished": {
            "label":"Unpublished",
            "description":"Visible only to Authors and Owners"
        },
        "published": {
            "label":"Published",
            "description":"Visible to all people with permissions"
        }
    } ...
}
```

States determine which Actions can be taken on a Stream at any given time.  This can be further limited to specific users so that, for instance, an Administrator might be allowed to "edit" a Stream when it is marked as "published", but the original commenter could not.


## Extending / Inheriting Templates

Many templates share are so closely related, or share a lot of features in common.  In this case, you can put common features in a base template that is "extended" by others.  

As an example, the default package includes an "article" template that defines *most* of the workflow for creating and publishing articles.  Two other templates -- "article-editorjs" and "article-markdown" -- extend this base template with specific functionality required for each of these different data types.

Here's how extensions work: If a template defines an action itself, then that is the action that is used.  Otherwise, Emissary searches through the list of extensions **in the order in which they are defined** to find that action name.  To extend other templates, add an "extends" key into your `template.json` file with an array of template names as its value.

**For example:** if a template declares this: `"extends": ["A", "B", "C"]` then Emissary will first search template "A" for the required actions, then "B", then "C".  

This inheritance structure can also cascade upwards, so that template "D" could extend "E", which itself extends "F".

This extension mechanism also makes it simple for Templates to inherit from multiple bases.  In this case, if there multiple extensions, then the first template listed (and all of it's ancestors in order) is searched first, then the second (with all of its ancestors), and so on.

