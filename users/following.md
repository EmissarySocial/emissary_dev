
Your Emissary inbox can follow a wide range of content online, giving you fast notifications of new posts from the people you want to hear from.  Emissary supports several different [protocols](#protocols), so your inbox can follow many more kinds of content than other Fediverse servers.

To follow someone, just go to your Profile \> Inbox and enter their fediverse handle or web address.  Emissary will look up their profile and connect to them with the best available .

## Folders

Emissary naturally organizes the accounts you follow into `Folders`, so you can group the posts you receive in any way you need to.  New posts will appear in their corresponding folders whenever they become available.

You can also change the appearance of each folder independently, using different layouts for each folder depending on the kind of content they usually post.

## Protocols

**Fediverse (ActivityPub)**  is the real-time messaging format that powers the Fediverse.  Emissary uses ActivityPub to send and receive real-time updates from other Fediverse servers.  ActivityPub is a rich format that supports many different operations (such as likes and boosts) and is the first choice to be used when available.

**RSS** is a widely-used standard that is supported by nearly every blog, podcast, and website builder.  It allows sites like Emissary to check periodically for new updates to specific content on the site.  Emissary uses RSS as a fallback when servers don't support ActivityPub, and will poll RSS feeds for new content once a day.

**WebSub** is an enhancement to RSS that provides real-time updates to RSS feeds.  Where available, Emissary will upgrade from RSS to WebSub in order to receive faster notifications from news sites.

## About "Global Feeds"

Emissary does not have a "global feed" that displays all content on the server.  Instead, only accounts that you explicitly follow will show up in your inbox.