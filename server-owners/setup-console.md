
The best way to run Emissary for the first time is to use the Setup Console.  This tool is built into the server's executable, and it gives you a simple GUI for initializing the configuration file and changing default settings.

To run the Emissary Setup Console, open a terminal window and navigate to the directory where you've downloaded the Emissary code, then enter the following command:

```
> go run server.go --setup
```

The setup console *should* open automatically in your desktop browser.  If it doesn't, just navigate to http://localhost:8080 to continue.  You can also use this mode to administer a remote Emissary instance, described below under Configuration Options.

**IMPORTANT: The setup console IS NOT PASSWORD PROTECTED** as it is intended to be run only from a secure administrator device.  Do not run this app mode on any publicly facing servers.

## Domains

In this section, you can create new Emissary sites and delegate admin rights to their owners.  To set up a new domain, you'll need:

* A unique domain name
* A MongoDB connection string and database name
* SMTP credentials (to send emails)
* Owner contact information

After you create a domain, you can also make additional admin accounts.  When a new accounts are created, they will automatically receive a welcome email that allows them to set up their own passwords on that domain.  You can also resend passwords from this console if they were not received.

## Server Settings
In the main navigation, "Server Settings" helps you set up the base capabilities for all domains on this server.

**Templates** define the different types of content that are available to the domains on this server -- [themes](/themes), [templates](/templates), and [widgets](/widgets).  Emissary comes packaged with a few default templates to get you started.  You can load additional templates from a number of different sources, including the **filesystem**, **Git Repositories**, and **S3 (compatible)** hosts.

**Email Templates** define the content of system email messages.  It works the same as the User Templates section.  If you define custom email templates, they must match the name used by the system templates exactly, or they will not be used to send emails.

**Attachments** is where you identify a writable file location where your users' uploaded files will be stored.  Emissary manages all uploads for you, and separates out upload locations based on each domain.  Options for upload locations include: 1) Any folder available through the OS filesystem, including local disks and NAS locations, 2) S3 (and compatible) URLs.

**Certificates** is where you identify a writable file location where SSL certificates are cached.  Emissary uses auto-generated certificates from [Let's Encrypt](https://letsencrypt.org) to encrypt every page request without any additional setup from you.

**ActivityStreams Cache** is where you configure a separate MongoDB database to serve as a shared cache for ActivityStream data from around the Internet.  This should be a trusted source because all domains will use the same data.  Only publicly available URLs are stored in this cache.