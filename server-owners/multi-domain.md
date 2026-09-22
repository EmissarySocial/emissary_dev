
Emissary is a multi-tenant system that can host more than one domain on a single server instance.  

However, each domain hosted on that server has its own dedicated database.  This makes it simpler to keep domains separated, as simplifying server migrations.

## Setup Console

The [Setup Console](https://emissary.dev/setup-console) makes it easy to add and remove domains from your Emissary installation.  To start the setup console, open your terminal and enter `go run server.go --setup`.  After the application compiles, your browser will open a new window with the setup console.

**Pro Gamer Move:** You can also make direct edits to the Emissary configuration file or configuration database.  Emissary watches its configuration for changes and will reload it when necessary.  Just make sure you're saving valid JSON or the new configuration will not be applied.

## Domains Tab

Click the "Domains" navigation option the setup console to list all of the domains currently hosted by this server.  You can add new domains to this list or edit existing ones.

You'll need to enter information about: the domain name, domain owner, MongoDB connection, and SMTP server.  SMTP servers are only used for specific transactional emails, such as sending and resetting users' passwords.

**Please Note** that DNS configuration is outside the scope of Emissary, so you'll need to make sure that the actual DNS records point to this server installation for the website to resolve correctly.

### Adding Users

When you add the domain, a welcome email is sent to the domain owner containing a link to their new Emissary instance.  Once the domain has been created, you can also add other users directly into the setup console by clicking the "Edit Users" button in that domain's listing.

When a user is created, they are automatically sent a welcome email (via the domain's configured SMTP server.  You can also resend emails in case there is a problem delivering them the first time. 

## Clustering

Large hosting environments, may want to provision more than one physical machine to serve a particular domain (or set of domains).  In this situation, it is best to set each physical server to use a central MongoDB database for configuration data, and then make configuration changes in a single place.  

**Please Note** Server provisioning, routing, and load balancer setup is outside the scope of Emissary, so you'll need to set these up to work with your hosted domains separately.  Once the network works, Emissary will run beautifully in your environment.