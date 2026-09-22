
Interactive maps are made by stitching together a grid of images, called map tiles.  Each tile covers a particular area of the world map. This format is sometimes called "Raster" maps (because the geographic information is "rasterized" into a static image) or "ZXY" tiles (because of the URL format used to retrieve map images).

To set up a custom map tile provider, sign in to your Emissary server as a domain owner, navigate to  **Server Settings \> External \> API Keys**.

Emissary natively supports a number of map tile providers.  You can also use custom map URLs if you have access to other, unspported maps.

## Recommended Providers

**[Geoapify](https://geoapify.com)** <br>
Geoapify is recommended because they support a wide range of other geoservices, so you will likely already have an API key.  They include a generous free tier that should support small websites.

## Additional Supported Providers

**[MapTiler](https://maptier.com)** <br>
MapTiler provides a number of high-quality maps, and includes a generous free tier that should support small websites.

**[Thunderforest](https://thunderforest.com)** <br>
Thunderforest provides a number of high-quality maps, and includes a generous free tier that should support small websites.

**[Open Street Map](https://openstreetmap.org)** <br>
Open Street Map is a free service that is used by default if no other provider is configured. They support standard maps in several languages.

## Custom Providers
In addition to the providers listed above, Emissary can use unlisted map tile providers.  Just select the `Custom` service provider and enter the correct "ZXY" URL template.  This URL template is usually similar to: `https://server.name/maps/{z}{x}/{y}.png`, and should be easily locatable on your map provider's website.  Emissary will use this template to locate the required map tiles by replacing the {z}, {x}, and {y} with the correct map coordinates.
