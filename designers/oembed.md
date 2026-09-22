
[oEmbed](https://oembed.com/#section4) is an open standard for embedding rich data into other web pages.  It's used by many social sites to provide nicely formatted previews of linked pages.

Emissary hosts an oEmbed compliant endpoint at `/.oembed` -- for example, check out [the oEmbed data for this page](/.oembed?url=https://emissary.dev/oembed)

## Standard oEmbed Previews

By default, Emissary includes the title and icon images for every Stream and User on the website.  Templates should aid in [oEmbed discovery](https://oembed.com/#section4) by including `<link>` tags that point to the `.OEmbedJSON` and `.OEmbedXML` URLs, like this:

```html
<link rel="alternate" type="application/json+oembed" href="{{.OEmbedJSON}}">
<link rel="alternate" type="text/xml+oembed" href="{{.OEmbedXML}}">
```

## Custom oEmbed Previews

In addition, you can also create a custom HTML template to generate rich oEmbed previews.  To do this, add an extra HTML file in the Template called `oembed.html`.  You don't need to create a new route.  This template uses all of the same values as any other template, but this HTML will be used by the oEmbed endpoint to generate a rich preview for all Streams that use this Template. 

For examples, please look at this [example oEmbed template](https://github.com/EmissarySocial/bandwagon/blob/main/bandwagon-album/oembed.html), and this [example oEmbed result](https://bandwagon.fm/.oembed?url=https://bandwagon.fm/6691ca55435feeeb4a0b4312) returned by the endpoint.