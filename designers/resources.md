
You can include static resources in any [Template](/templates) or [Theme](/themes).  Just add a subdirectory named **resources** to the theme or template folder, and place your static files there.

| Container     | URL                                                 |
| ------------- | --------------------------------------------------- |
| For themes    | `/.themes/<theme-name>/resources/<file-name>`       |
| For templates | `/.templates/<template-name>/resources/<file-name>` |

<br>

Resource folders are primarily used for images, fonts, and other binary objects.  To serve text files such as Javascript as CSS, look first at [Bundles](/bundles), which allow you to combine multiple text files into a single download.

Because resource folders can be defined separately in every theme and template, you can be confident that your code will not trample on other unrelated plugins, and vice versa.

To minimize bandwidth, Emissary automatically adds caching headers to all served resources.