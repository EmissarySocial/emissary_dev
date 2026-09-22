
Follow this walkthrough to install and configure a basic Emissary server on your local machine.  Visit the  [configuration](config) for more complete instructions.

<iframe title="Emissary Quickstart" class="width-100-percent aspect-16-9 margin-bottom"  src="https://clip.place/videos/embed/9YaSZyNmDXhBbR4qoWbP1L" frameborder="0" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups allow-forms"></iframe>

## Prerequisites

[Go compiler](https://go.dev) - Download and install the Go compiler.  You'll need version 1.21 or higher to compile and run Emissary.

[MongoDB database](https://mongodb.org) - Access to a mongodb database.  You  can download and install this locally, or sign up of a number of services that will host one for you, such as MongoDB Atlas, Digital Ocean, A2 Hosting, or Kamatera.

[FFmpeg](https://ffmpeg.org/download.html) - The FFmpeg media library must be installed on the host machine for Emissary to resize images, and transcode all kinds of media files. FFmpeg is included in the Docker container definitions.

[Emissary Source Code](https://github.com/EmissarySocial/emissary) - Check out the source code via Git, or download a .ZIP archive directly from GitHub.

## Configure MongoDB Replica Set

Emissary uses important features of MongoDB that are only available in [replica set](https://www.mongodb.com/docs/manual/replication/) deployments. This is a common way to configure clusters of MongoDB servers, but if you're only using a single MongoDB installation (for instance, in a development or test environment) then you'll need to take some extra steps.

Converting a single server is a one-time, low-overhead change: give `mongod` a `replSetName`, restart it, and run `rs.initiate()` once. See MongoDB's tutorials for
[converting a standalone to a replica set](https://www.mongodb.com/docs/manual/tutorial/convert-standalone-to-replica-set/),
[deploying a single-node replica set for testing and development](https://www.mongodb.com/docs/manual/tutorial/deploy-replica-set-for-testing/).

When connecting to a single-node replica set from outside its own network (for example, a local Docker container), append `?directConnection=true` to your connection string so the driver doesn't hang trying to discover other members.


## Install Emissary

In your terminal, navigate to the directory where you extracted the Emissary source code, then enter this command to start the configuration console: 

`> go run server.go --setup`

The setup console should open in your desktop browser.  If it does not, just open a browser and navigate to http://localhost:8080 to begin.  By default, this will create and edit a file called **config.json** in the current folder.  There are many other startup and configuration settings that we will skip over for this quickstart.  Later, you can dig into more details in the [the Configuring section](/configuring).

### Server Settings

Before you set up a new domain, you'll need to choose "Server Settings" in the top navigation bar to enter some global defaults.  On the "General" tab, enter the connection string for your MongoDB database, along with a distinct database name that will be used for common services.  This will power the shared cache and queue services that can be used by every domain on the server.

**Common Services**
Emissary uses a shared database to manage common services, such as a shared ActivityStream cache.  To run this server, you'll need to enter MongoDB connection details for this shared database.

**Ports**
By default, the Emissary server listens to port 8080 for incoming HTTP connections.  You can change these values to fit your own needs.

**Testing and Development**
If you need to troubleshoot behavior, or if you want to see what Emissary is doing under the hood, you can turn on several levels of debugging output.

**Other Tabs**
There are other settings here that we can ignore for now.  The default locations for other resources will work fine for this quickstart.

### Domain Settings
Choose "Domains" in the top navigation bar, then click "Add a Domain" and then fill in the pop-up with your test domain info:

```
Label: My Test Domain
Hostname: localhost
MongoDB Connection String: <connection string for your mongodb database>
MongoDB Database Name: <name of a new, empty database to populate> 

Account Owner: Your name, site username, and email
```

**SMTP Server (production only)**

On local domains (like "localhost", "x.local", "192.168.x.x") server administrators can edit usernames and passwords.  If you're just testing out Emissary or developing new templates, an SMTP server optional, but not required.  

On production domains, server administrators CAN NOT enter users' passwords, so you'll need to enter SMTP server address and credentials. This will allow Emissary to send transactional emails for things like user welcome message and password resets.

**IMPORTANT** The setup server IS NOT PASSWORD PROTECTED, and you should never run it on a publicly available server.

## Run Emissary

Open a new terminal on your computer, navigate to the location of the Emissary source code, and enter the following command to run Emissary in live mode: `> go run server.go`  

Your website *should* be running now.  If you haven't checked yet, check your email for your welcome message.  If you haven't received it, go back to the setup console and verify that it was sent correctly.

Once you receive your welcome email, click the link to open up your test website at http://localhost.  Enter your new password and then you'll see the setup wizard, which guides new site owners through the rest of the setup process.

## Explore

Once you set up your password, you'll get to choose a [Theme](/themes) and start using Emissary.  See what it's like to create and edit new pages, or to follow your favorite social media channels in the inbox.

Welcome! You've just taken your first steps into a larger world.  The rest of this section will give you more detailed instructions on running more customized versions of the Emissary server.