
In December 2025, the [Social Web Foundation](https://socialwebfoundation.org) launched a project to build End-to-End-Encryption (E2EE) into ActivityPub and the Fediverse, with funding from [The Soveriegn Tech Fund](https://www.sovereign.tech/).  Here's the [the official project announcement](https://socialwebfoundation.org/2025/12/19/implementing-encrypted-messaging-over-activitypub/). 

Emissary is proud to be one of the servers chosen for this work.  We are aiming to deliver the first version of an encrypted messenger in mid 2026.

In broad terms, we're using Messaging Layer Security (MLS) Protocol to manage keys and encrypt data locally on users' devices.  MLS is an open, flexible industry standard has is being used "at scale" by a number of large organizations.  We're now implementing this in ActivityPub, using our existing servers to provide two important services to the MLS protocol:

1. MLS "directory service" to locate users and their encryption keys (WebFinger, Actor profiles)
2. MLS "delivery service" to send encrypted messages between clients (ActivityPub inboxes and outboxes)

Here is my list of the online resources that are helping me to understand the MLS protocol and how it is used within ActivityPub.

## Official Specifications

* **[Messaging Layer Security in ActivityPub](https://swicg.github.io/activitypub-e2ee/mls)** describes how to use MLS in ActivityPub
* **[RFC 9420](https://www.rfc-editor.org/rfc/rfc9420.html)** is the primary MLS specification from the IETF
* **[SWICG Github Project](https://github.com/swicg/activitypub-e2ee)** where we're organizing our work and [discussing issues](https://github.com/swicg/activitypub-e2ee/issues)

### Related Specs

* **[Server Sent Events for the ActivityPub API](https://swicg.github.io/activitypub-api/sse)**

## Tutorials

* **[Phoenix R&D Blog](https://blog.phnx.im/rfc-9420-mls/)** has a good introduction to the MLS protocol
* **[Positive Intentions Blog](https://positive-intentions.com/blog/mls-group-messaging/)** has a deep dive on building MLS applications (with Typescript code!)
* **[Demystifying MLS](https://wire.com/en/blog/messaging-layer-security-mls-explained)** by Wire, one of the original participants in creating the MLS spec

## Audio/Video Tutorials
* **[Podcast with Raphael Robers](https://securitycryptographywhatever.com/2023/04/22/mls/)** that I still need to listen to
* **[Video Presentation by Konrad Kohbrok and Raphael Robert](https://media.ccc.de/v/37c3-12064-rfc_9420_or_how_to_scale_end-to-end_encryption_with_messaging_layer_security)** that I still need to watch
* **[YouTube Video by Chalk Talk](https://www.youtube.com/watch?v=FESp2LHd42U)** that I still need to wath

## Libraries

* **[ts-mls](https://github.com/LukaJCB/ts-mls)** is a Typescript library for the MLS protocol, and is the library I'm using for Emissary
* **[OpenMLS](https://openmls.tech)** is a Rust library for the MLS protocol


## Project Plan

If you're following along from home, here's how I'm tackling this problem and building now.

Building E2EE into Emissary is tricky.  As a programmable ActivityPub server, Emissary uses server-rendered HTML templates for everything -- even outbound JSON-LD documents pass through server-based filters.  So, encrypting messages on the browser client will require a number of new technologies to be built into Emissary:

* **ActivityPub API Inbox/Outbox** - So far, Emissary hasn't needed to support the client-facing ActivityPub API.  So, I'm reworking the [hannibal library] to better support this, and building a true ActivityPub inbox/outbox into Emissary.  This also paves the way for future work to support other client-to-server (C2S) interactions on Emissary.

* **Support for new MLS message types** - Hannibal has also been updated to recognize encrypted MLS messages when they reach the server.  Their contents are opaque to the Emissary server, but we now know that they exist and can route them to users' client apps correctly.

* **Browser Client App** - I've made a proof-of-concept using [Mithril.js](https://mithril.js.org) and [ts-mls](https://github.com/LukaJCB/ts-mls) that places a fully client-based "conversations" app alongside Emissary's server-side "inbox" app.  I've used Mithril in the past, and appreciate its light weight and fast design as compared to heavier JS frameworks like React.  The client-side app is built with [esbuild](https://esbuild.github.io) so that we can also use [JSX templates](https://mithril.js.org/jsx.html) in the Mithril application.  It's looking really nice.

## Project Status

**2026-04-16** I haven't added info here for a while, but have been making *significant progress* on the app. Here are some highlights:

* Group metadata (name, notes, tags) is synchronized privately among my own devices, but not shared with other group members.
* Send and display "message received" acknowledgements
* Sending InReplyTo links 
* Send and display emoji reactions
* Large group conversations (I've tested it with 26 group members). 
* You can edit and delete messages, and view the edit history. 

Also, here's [a recent project conversation with a progress screenshot](https://mastodon.social/@benpate/116403692756660367). I've worked really hard to make a clean, consistent UX for this, and I'm eager to see how it works for other people.

We are rapidly approaching our third project milestone at the end of the month, when most chat features are due. Remaining on my list is: improving key rotation, image attachments, improved setup and sign-in.  I also hope to deliver plaintext messaging along with this, but that's not a requirement for this SWF E2EE project.

**2026-02-23** [Published demo video](https://clip.place/w/ajgJ5Hi69bbbxHCK3nXNdj) showing E2EE messages working between two different servers, messages, replies, and key management.

**2026-02-21** E2EE client now lives in [its own separate repository](https://github.com/EmissarySocial/conversations-mls).  Before launch, I'll work out a clean way to embed this into the primary Emissary distribution.

**2026-02-20** - Real time notifications with SSE are now working. Fortunately, this wasn't too hard because Emissary already supported several channels for SSE updates.  What's cool, however, is that this is the first SSE channel that implements the [Server Sent Events for the ActivityPub API](https://swicg.github.io/activitypub-api/sse) specification, which will be a big move forward for ActivityPub clients.

**2026-02-16** [Thanks Luka](https://github.com/LukaJCB) for your help troubleshooting encoding issues.  We're now successfully receiving and decoding MLS-encrypted messages.

**2026-02-06** We're fully migrated to a release candidate for ts-mls v2.0.  Also, fun progress today on the UX.  The [mithril.js front end](https://mithril.js.org) is now correctly displaying groups and messages.

(I spent this whole week sick in bed with the FOSDEM flu. Don't judge me)

**2026-02-05** Two steps forward, one step back. Coming back from FOSDEM, I'm working to update to [ts-mls v2.0](https://github.com/LukaJCB/ts-mls).  It has a much improved API, but it still represents work to make the change.  I'm also making some progress on the overall architecture, and management of various conversations, or "groups" saved on your browser.

**2026-01-13** Big exciting milestone today, with the first encrypted messages sent (but not received) via MLS. [Check out my "toot" for more details](https://mastodon.social/@benpate/115890374883829695) :)

**2026-01-06** Working to integrate features of the MLS demo app.  And, I've made some progress on modeling the various services that this thing is going to need. It's slow going, because there's a lot to ingest.  However, once I get past a few more thorny issues, I'm expecting to repurpose large sections of the app quickly :)

**2026-01-05** Modeling new apps from scratch is fun, but hard. There are so many ways it could go, but I have to pick the one it will go.. at least for now. Now that I have a “nearly working” outbox mechanism (with more still due, unfortunately) I’ve turned to face the actual MLS portion of this. I’m hoping to have some rudimentary encryption working in the next few days.

**2026-01-03** I'm working with [a demonstration MLS app](https://github.com/positive-intentions/cryptography/) featured on the [positive intentions blog](https://positive-intentions.com/blog/mls-group-messaging) as a baseline for the MLS integration. It looks really promising.

**2026-01-02** The "conversations" app is coming along. I can send plaintext messages to the server, and am working to populate Conversation objects and route messages to the correct recipients.

