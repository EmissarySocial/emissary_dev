
Widgets are small, additional templates that can be mixed into a Stream in one or more places.  The specific locations are defined by the Template, and can appear anywhere within the Stream's HTML, but are most commonly TOP, BOTTOM, LEFT, and RIGHT.

Each of these regions can include zero or more Widgets, each adding specific functionality to the Stream's page.  For example, Emissary ships with several default widgets for navigation menus, semantic page titles, bylines, and breadcrumbs.

## Creating and Distributing Widgets

Each Widget is just a collection of files in a directory that Emissary knows to scan. This can be a directory in the local filesystem, or a remote Git repository that is registered in the server configuration. Emissary will recognize a directory as a widget if it contains a `widget.json` definition file.

### Widget File Format

| Property    | Description                                                                        |
| ----------- | ---------------------------------------------------------------------------------- |
| widgetId    | unique identifier of this widget                                                   |
| label       | Human-friendly short description to display in lists                               |
| description | Human-friendly long description to display in lists                                |
| bundles     | A set of [bundle definitions](/bundles) for files included with this widget        |
| schema      | A validating [schema](/schemas) that defines the custom data that this widget uses |
| form        | A [form](/forms) used for editing custom settings in this widget                   |

### Widget HTML

Widgets are much simpler than stream templates, in that they do not have multiple views or actions.  Instead, each widget is expected to be a relatively simple piece of code that is affixed to the edges of the main stream content.  So, each widget only requires a single HTML template in its folder, named `widget.html`.  Additional HTML templates can also be defined within the widget, but they need to be embedded within the main HTML code.

## Using Widgets in a Template

Because template layouts each have their own considerations, templates themselves are responsible for actually placing widgets into a stream page.  To to this, the template lists one or more location names where it allows templates in the `widgetLocations` property of it's JSON definition file.  Standard values are TOP, BOTTOM, LEFT, and RIGHT.  But, custom templates can choose any values that make sense to their design.

Next, the template's "view" action must add code to check for widgets in each location and apply them to the page HTML.  The [built in Article Template](https://github.com/EmissarySocial/emissary/blob/main/_embed/templates/stream-article/view.html) contains a good example of this.

Last, the template's "edit" section must add a section for choosing and arranging the templates.  Emissary includes a base template called `base-widget-editor` that does most of this work for you.  You can see how it's implemented the [built-in Article template](https://github.com/EmissarySocial/emissary/blob/main/_embed/templates/stream-article/template.json).