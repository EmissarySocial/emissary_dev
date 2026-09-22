To interoperate with other applications on the Fediverse, Emissary implements a number of protocols in addition to ActivityPub, which are listed below for reference.

* [ActivityPub](https://www.w3.org/TR/activitypub/) Emissary supports ActivityPub federation with a wide range of servers.
* [RSS / Atom / JSONFeed](https://en.wikipedia.org/wiki/RSS) Emissary generates RSS feeds, and can subscribe to RSS feeds.
* [OpenGraph](https://ogp.me) Emissary reads OpenGraph metadata from pages that do not include ActivityPub or RSS data. In addition, Emissary's templates *should* present OpenGraph data to clients.
* [WebSub](https://www.w3.org/TR/websub/) Emissary can send and receive content notifications via WebSub
* [WebMentions](https://www.w3.org/TR/webmention/) Emissary can send and receive WebMentions
* [OAuth 2.0](https://oauth.net) Emissary is an OAuth 2.0 server
* [Mastodon API](https://docs.joinmastodon.org/client/intro/) Emissary supports a subset of the Mastodon API which lets you use third-party Mastodon clients to post to your Emissary site. (in progress)
* [oEmbed](https://oembed.com) Emissary published oEmbed data so that other platforms can provide rich, [custom links](/oembed) to pages on emissary.

### Fediverse FEPs

* [Blocked Collection (FEP-C648)](https://codeberg.org/fediverse/fep/src/branch/main/fep/c648/fep-c648.md)
* [Group Federation (FEP-1B12)](https://codeberg.org/fediverse/fep/src/branch/main/fep/1b12/fep-1b12.md)
* [FEDERATION.md (FEP-67FF)](https://codeberg.org/fediverse/fep/src/branch/main/fep/67ff/fep-67ff.md)
* [Activity Intents (FEP-3B86)](https://codeberg.org/fediverse/fep/src/branch/main/fep/3b86/fep-3b86.md)

### Well-Known URLs

* [Host-Meta (RFC 6415)](https://datatracker.ietf.org/doc/html/rfc6415)
* [Node Info 2.0 and 2.1](http://nodeinfo.diaspora.software)
* [WebFinger (RFC 7033)](https://datatracker.ietf.org/doc/html/rfc7033)
* [Change-Password](https://www.w3.org/TR/change-password-url/)
