
Building on the Fediverse is hard.  Here is a list of ActivityPub and other developer resources that have been helpful in developing Emissary. This is not a canonical or official list by any means, but hopefully this list of bookmarks is valuable to others who are building their own Fediverse apps.

Life comes at you fast. Stuff may have changed since this page was updated **November 2025**.

## Official Specifications

* **[ActivityPub](https://www.w3.org/TR/activitypub/)** is the protocol for sending messages between Actors' inboxes and outboxes.  This spec does not work on its own, but requires apps to implement many other specs and protocols in order to function on the Fediverse.
* **[ActivityStreams 2.0](https://www.w3.org/TR/activitystreams-core/)** specifies the JSON format for messages sent via ActivityPub, along with documents available for download via users' profiles and outboxes.
* **[Activity Vocabulary](https://www.w3.org/TR/activitystreams-vocabulary/)** specifies the vocabulary terms that appear in an ActivityStream document.  This is a very useful map to understanding how ActivityPub is used, but remember that these terms are loosely defined, and may be used differently by different applications.
* **[WebFinger](https://webfinger.net)** is a straightforward discovery protocol that is required to integrate with most Fediverse applications.  Emissary supports this with the [digit library](https://github.com/benpate/digit)
* **[HTTP Signatures (draft Cavage)](https://datatracker.ietf.org/doc/draft-cavage-http-signatures/12/)** are required by *most* Fediverse apps in order to authenticate messages.  This draft is an older version of the specification that is currently in use by most Fediverse Apps.  Emissary supports this with the [hannibal library](https://github.com/benpate/hannibal)
* **[HTTP Signatures (current)](https://datatracker.ietf.org/doc/rfc9421/)** is the newest version of the HTTP signatures specification that has been finalized after many years in draft form.  As of spring 2024, I don't know how well supported this is.
* **[OAuth](https://oauth.net)** is commonly used to generate API tokens that allow users and apps to sign in to Fediverse servers.
* **[NodeInfo](http://nodeinfo.diaspora.software)** and **[Host-Meta](https://datatracker.ietf.org/doc/html/rfc6415)** are meta-data formats that publish statistics about a server's software and community.  They're not exactly *required* to send messages to other servers, but is a good idea to include.

## Tutorials

* **[Guide for New ActivityPub Implementors](https://socialhub.activitypub.rocks/t/guide-for-new-activitypub-implementers/479)** is a good high-level tour of the ActivityPub ecosystem.
* **[ActivityPub Primer](https://www.w3.org/wiki/ActivityPub/Primer)** The W3C Wiki includes a series of articles detailing how many ActivityPub messages should be sent and received.  It's a good starting place for understanding ActivityPub development.
* **[Understanding ActivityPub](https://seb.jambor.dev/posts/understanding-activitypub/)** is an extensive tutorial for understanding how ActivityPub works in the real world.
* **[Signing HTTP Messages](https://justinsecurity.medium.com/signing-http-messages-962510d65895)** walks you through creating, verifying, and testing HTTP signatures.
* **[Remote Follows](https://www.hughrundle.net/how-to-implement-remote-following-for-your-activitypub-project/)** are one of the cool, older Fediverse features that have been left behind by the documentation. This is a really good tutorial on how to implement them.
* **[Mastodon Preview Cards](https://box464.com/posts/mastodon-preview-cards/)** tutorial on the data Mastodon uses to generate preview cards for linked pages.

## Fediverse Community

*  **[FediForum](https://fediforum.org)** is a semi-annual online un-conference dedicated to all things Fediverse.  Usually March and September.  You should attend.
*  **[IFTAS](https://about.iftas.org)** is dedicated to "Federated Trust and Safety" by providing tools and resources to site moderators to keep all that junk off the Fediverse.
*  **[FediDB](https://fedidb.org)** Networks statistics across the whole Fediverse -- or as much of it as we know about.
*  **[How to be a Good Fediveres Citizen](https://stefanbohacek.com/blog/how-to-be-a-good-fediverse-citizen/)** more notes on common expectations of Fedizens.
*  **[Privacy and Consent](https://wedistribute.org/2024/07/fediverse-privacy-and-consent/)** for Fediverse developers is a tough subject.  WeDistribute collects some best practices.


## Developer Community

* **[FediDevs](https://fedidevs.org)** is the central developer group for Fediverse developers.  If you're building ActivityPub apps, get involved here.
* **[ActivityPub Rocks](https://activitypub.rocks)** is an older hub for ActivityPub development.  It has some good introductory information about building ActivityPub apps, but it has not been updated in quite some time, so it should not be trusted for details on current specs or implementations.
* **[SocialHub](https://socialhub.activitypub.rocks)** is an active community forum that host many Fediverse developer discussions
*  **[Fediverse Ideas](https://codeberg.org/fediverse/fediverse-ideas)** is a Git repository to discuss ideas that are not ready for a formal FEP.
*  **[W3C SWICG](https://github.com/swicg)** is the standards body doing current development on ActivityPub and its implementations.
*  **[Fediverse Enhancement Proposals (FEPs)](https://codeberg.org/fediverse/fep)** is a Git repository of proposed enhancements to Fediverse specifications.  Applications will often list the FEPs that they support.  Example: [Emissary's FEP support list](https://emissary.dev/protocols)

### FEPs to Watch
Speaking of Fediverse Enhancement Proposals (FEPs), here is an (incomplete) list of some that I think you should check out.

* **[FEP-3b86 - Activity Intents](http://w3id.org/fep/3b86)** allows servers to publish URL templates for remote access to various ActivityPub activities.
* **[FEP-ef61 - Portable Objects](https://codeberg.org/fediverse/fep/src/branch/main/fep/ef61/fep-ef61.md)** is a forthcoming account portability standard
* **[FEP-7888 - Context Property](https://codeberg.org/fediverse/fep/src/branch/main/fep/7888/fep-7888.md)** is used by NodeBB, decodon, and others to enable threaded discussions.
* **[FEP-c390 - Identity Proofs](https://codeberg.org/fediverse/fep/src/branch/main/fep/c390/fep-c390.md)** Used by Hubzilla to provide nomadic identity.
* **[FEP-d8c2 - OAuth Profiles](https://w3id.org/fep/d8c2)** identifies a way for ActivityPub servers to use OAuth without defining a client\_id ahead of time.

## Developer Resources

* **[Delightful Fediverse Development](https://delightful.coding.social/delightful-fediverse-development)** is a curated list of developer tools across a wide variety of languages and environments, and a part of the [Delightful Commons](https://delightful.coding.social) resource lists.  If you're looking for libraries to help you get started, check out this list first.
* **[Delightful Fediverse Clients](https://delightful.coding.social/delightful-fediverse-clients/)** is a curated list of applications on the Fediverse, and a part of the [Delightful Commons](https://delightful.coding.social) resource lists.  It is pretty comprehensive, though it contains many apps that have long been abandoned.
* **[ActivityPub.academy](https://activitypub.academy)** is a modified Mastodon instance that lets you create a test account, then watch the ActivityPub messages sent/received as you interact with other Fediverse apps.
* **[HTTP Signatures Demo](https://httpsig.org)** demonstrates both signing and verifying HTTP signatures.

## Testing Tools
Unfortunately, there are no *perfect* tools for testing ActivityPub applications at the moment.  But, there are several projects in the works, so hopefuly this will change in the near future.  Here's what I'm aware of currently.

* **[Fun Fediverse Development](https://funfedi.dev/)** is doing a lot of great work, including an online [actor verification](https://verify.funfedi.dev/) tool, a set of [docker images](https://funfedi.dev/fediverse_pasture/) of popular servers to test against, and [support tables](https://funfedi.dev/support_tables/) to test how your application will process data from different servers.
* **[ActivityPub Academy](https://activitypub.academy)**, mentioned above, allows you to trace actual ActivityPub messages with a live Mastodon server.
* **[Fediverse Schema Observatory](https://observatory.cyber.harvard.edu)** is a fantastic tool that catalogs the many disparate data formats used by all of the different Fediverse apps online.
* **[Browser Pub](https://browser.pub)** is a fantastic debugging tool that loads and analyzes ActivityPub/ActivityStream objects.
* **[Feditest](https://feditest.org)** is an ongoing effort to build a reliable testing framework that spans all Fediverse apps.  Watch this space!
* **[PEM Parser](https://8gwifi.org/PemParserFunctions.jsp)** validates public keys are correctly encoded
*  **[JSON-LD Playground](https://json-ld.org/playground/)** is a robust online validator that verifies JSON-LD documents.
*  **[JSON lint](https://jsonlint.com)** is a simple online JSON validator to check that your JSON is formatted correctly.
*  **[OpenGraph.xyz](https://www.opengraph.xyz/)** previews the [OpenGraph](https://ogp.me) data embedded in your web pages.

## Go Resources
Here are the libraries that I have considered and used to build ActivityPub support in Emissary.

* **[Hannibal](https://github.com/benpate/hannibal)** was built for Emissary.  It includes implementations for activitystreams documents, vocabulary, inbox, outbox, and http signatures.  Hannibal is fast-and-loose with the standards (especially ActivityStreams @context) but it is easy to use and smoothes over the rough edges of Fediverse development.
* **[Digit](https://github.com/benpate/digit)** implements WebFinger objects in Go.  You'll still need to make HTTP handlers to server WebFinger requests, but this makes the process pretty simple.
* **[Toot](https://github.com/benpate/toot)** implements the Mastodon server API, allowing Emissary to work with clients that are intended for Mastodon. (in progress)
* **[Sherlock](https://github.com/benpate/sherlock)** was built for Emissary, but is the oddball in this space.  This library looks up metadata for any web page (or user profile) and then returns a faked ActivityStream representation of the resource.  It makes every page on the Internet *look like* it supports ActivityStreams, even if it does not.

* **[Go-Fed](https://github.com/go-fed)** is the original ActivityPub framework for Go, and is used by several apps.  I evaluated this a couple of times, but it was too complicated for me.  It relies on code generation to enumerate all of the possible actions that can be taken on a document, which leads to some [absolutely enormous objects](https://pkg.go.dev/github.com/go-fed/activity@v1.0.0/streams).
* **[Go-AP](https://github.com/go-ap)** is a newer ActivityPub framework that seems much more approachable than go-fed.  I also evaluated this one, but ultimately built my own so that I could understand what was happening under the hood.
