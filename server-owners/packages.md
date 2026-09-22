
Packages contain the various behaviors available in each Emissary installation.  Package themselves can contain any number of [Themes](/themes), [Templates](/templates), and [Widgets](/widgets) that all work together to implement a complete web application.

To keep installations small and simple, and to help administrators maintain control of their servers, site owners cannot control what features are available on their website.  Instead, packages are controlled by the server administrator.  If you have specific needs that are not being met by your server administrator, then you'll need to host your own instance of Emissary.

## Setup Console

The [Setup Console](https://emissary.dev/setup-console) makes it easy to add and remove packages from your Emissary installation.  To start the setup console, open your terminal and run `go run server.go --setup`.  After the application compiles, your browser will open a new window with the setup console.

**Pro Gamer Move:** You can also make direct edits to the Emissary configuration file or configuration database.  Emissary watches its configuration for changes and will reload it when necessary.  Just make sure you're saving valid JSON or the new configuration will not be applied.

## Server Settings

To make changes, click on the "Server Settings" option in the navigation bar of the Setup Console.  You'll see a list of all the packages available in your Emissary installation.

## Default Package

Emissary ships with a default set of content embedded in its code.  When you first run the setup console, this default location are listed as an "EMBED" type.  You can remove this default, but be careful.  If you have not added other custom packages into the system to take their places, then it will become unstable or unusable.

## Additional Packages

To add another package location, click the "Add a Row" link and fill in its details.  These can come from many different locations:

* Embedded files (discussed above)
* Local Filesystem
* Network Attached Filesystem (NAS)
* Any Git provider, including GitHub, GitLab, and others.

Just choose the storage type, fill in additional connection information, and Emissary will automatically scan and import the Themes, Templates, and Widgets stored with that provider.