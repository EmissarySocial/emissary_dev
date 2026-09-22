
Emissary is designed to work in a wide range of environments, so you have several options for bootstrapping the application on your servers.  The core configuration required to run an Emissary server provides answers to three basic questions:
* What domains should I respond to?
* Where are my templates stored?
* Where do I save files, like uploads and SSL certificates.

This core configuration data can be stored in the filesystem (either a local disk or network attached) or in a database.

---
## 1. Bootstrapping from the Filesystem

By default, when you run Emissary from the command line with no additional options, it will look for a configuration file in the local directory named *config.json*.

### Custom File Locations

You can specify a different filename or location for your configuration data by starting emissary with a command line option like this: `% emissary --config=file://path/to/config.json`.  Emissary will load its configuration data from a local drive, NAS, or any other path available to your server's filesystem.

### Real-Time Updates
While running, Emissary will watch the file for changes, so if you (or some other external process) update it out-of-process, those changes will be reflected in your live server instantly.

### Setup Console
If you are running Emissary for the first time, you should also include a `--setup` flag in the command line to open the [Setup Console](/setup-console), which will initialize a new configuration file for you and give you an easy way to modify the default settings.

---
## 2. Bootstrapping from a Database

In some circumstances, you may not want to store your configuration in the filesystem, local or otherwise.  In this event, Emissary lets you pull configuration from a mongodb database. Just start the server with a MongoDB connection string like this:`% emissary --config=mongodb://connection-string-with-username-password`.

Emissary will open the MongoDB installation and look for a database named "Emissary" that should contain its domain and file location data.  The server will not run if this database and collection do not exist.  If you are connecting to a database for the first time, add the `--setup` flag to the command line to launch the Setup Console.

### Real-Time Updates
Just like the filesystem configuration, Emissary will watch the database for changes, and will reload the system configuration in real-time for every update.

### Clustering With Database Configuration

The best way to run a cluster of Emissary servers is to point each one of them at the same MongoDB database.  That way, you can easily provision new domains and have them respond across all servers in the cluster instantly.

---
## 3. Using Environment Variables

In many cloud-based hosting environments, you may not be able to specify a command line argument.  In this case, you can use an environment variables instead of a command line parameter.  To locate the configuration file in this way, first create an environment variable called EMISSARY\_CONFIG:

```
EMISSARY_CONFIG = file:// or mongodb:// location 
```

Then, you can launch Emissary directly:

```
% emissary (no config path required)
```
