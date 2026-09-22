
Bundles package static text resources (such as CSS, Javascript, and Hyperscript) into a [Theme](/themes), [Template](/templates), or [Widget](/widgets).

All of the files in a bundle are concatenated into a single download, minified according to their content-type, and served to client browsers as a single download with optimized HTTP headers.  They consist of two parts, an entry in the JSON definition file, and a sub-directory containing additional files.  Because bundles are defined separately at each of these levels, you can be confident that your code will not trample on other unrelated plugins, and vice versa.

### JSON Definition File

To create a bundle, add a "bundles" item into the JSON definition file, keys are the bundle names, which are used to download the bundle from the server.  Each bundle object in this JSON contains a single item, "content-type" which identifies the MIME Type of the content.  This is used in the HTTP header of the resulting file, and is also used to minify the results.  If you use a content-type of "text/css" or "application/javascript" then Emissary will use the appropriate minifier to greatly reduce the size of the resulting download.

Here is an example:

```json
{
	"bundles": {
		"stylesheet": {
			"contentType":"text/css"
		},
		"javascript": {
			"contentType":"text/javascript"
		},
		"hyperscript": {
			"contentType":"text/hyperscript"
		},
		"registration": {
			"contentType":"text/hyperscript"
		}
	}
}
```

### Sub-Directory

Next, create a sub-directory within the Theme, Template, or Widget with the same name as the bundle.  You can populate any number of text files within this directory.  On request, Emissary will concatenate these files into a single download and minify them (if possible) to reduce the overall download size.

## Using Bundles

Emissary includes several magic, hidden URLs to download bundles according to the type of container.

| Container     | URL                                         |
| ------------- | ------------------------------------------- |
| For themes    | `/.themes/<theme-name>/<bundle-name>`       |
| For templates | `/.templates/<template-name>/<bundle-name>` |
| For widgets   | `/.widgets/<widget-name>/<bundle-name>`     |

<br>

Bundles can be used anywhere you need a URL, such as:

`<link rel="stylesheet" href="/.themes/default/stylesheet">`

or

`<script type="application/javascript" src="/.templates/article/javascript"></script>`