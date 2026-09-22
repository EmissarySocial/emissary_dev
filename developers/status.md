Emissary is a work in progress, but we're making incredible progress.  Here are some of high-level goals for the initial version, and where we are on each of them.  For a more detailed view, you're welcome to come visit the [project kanban](https://trello.com/b/Ir9dDTdu/emissary-dev) and [GitHub page](https://github.com/EmissarySocial/emissary).

## Content: Core

| Title                        | Description                                            | Status |
| ---------------------------- | ------------------------------------------------------ | ------ |
| Create and Share             | Making a website is fast and easy                      | READY  |
| Private Group Sites          | Make a collaborative site with many users              | READY  |
| Themes                       | Swappable themes for every domain                      | READY  |
| Templates                    | Custom, downloadable templates for every content type  | READY  |
| Widgets                      | Interchangeable widgets to mark up pages               | READY  |
| Workflow                     | Customizable workflow rules based on swappable actions | READY  |
| Image Uploads                | Upload and transcode images                            | READY  |
| Supported Storage Providers  | Local Filesystem, AWS                                  | READY  |
| Security                     | Simple access settings for most cases                  | READY  |
| Audio Uploads                | Upload and transcode audio files                       | READY  |
| Video Uploads                | Upload and transcode video files                       | -      |
| Additional Storage Providers | Azure, Google, Dropbox                                 | -      |


## Content: Templates

| Title                             | Description                                                               | Status  |
| --------------------------------- | ------------------------------------------------------------------------- | ------- |
| [Bandwagon](https://bandwagon.fm) | Audio sharing for Bands and Musicians                                     | READY   |
| WYSIWIG Editor                    | Uses block based [Editor.js](https://editorjs.io)                         | READY   |
| Markdown Editor                   | Uses [Easy-MDE](https://github.com/Ionaru/easy-markdown-editor)           | READY   |
| Folders                           | Display child streams in several layouts.  For Blogs/Podcasts/Directories | PREVIEW |
| Photo Gallery                     | Displays photos in a few different formats.                               | PREVIEW |
| Chat/Forum                        | Realtime chat room/forum features using streams.                          | -       |


## Social: General

| Title            | Description                                                    | Status |
| ---------------- | -------------------------------------------------------------- | ------ |
| Post             | Create and edit new posts in your timeline                     | READY  |
| Follow           | Follow others' posts                                           | READY  |
| Block            | Block malicious or abusive users                               | READY  |
| Syndicate In     | Pull posts from other networks into Emissary                   | READY  |
| Syndicate Out    | Forward posts from Emissary to other networks                  | READY  |
| Profiles+        | Like site themes, allow users to choose their profile template | -      |
| Private Mentions | Send and receive private messages with other server            | -      |
| WebRTC           | Private Video Conferencing                                     | -      |


## Social: RSS Reader

| Title           | Description                                                                 | Status |
| --------------- | --------------------------------------------------------------------------- | ------ |
| RSS Poll        | Poll RSS Feeds for updates                                                  | READY  |
| WebSub Basic    | Realtime push via WebSub                                                    | READY  |
| WebSub Advanced | [Use WebSub "fat pings"](https://trello.com/c/oH8CDE6x/20-websub-fat-pings) | -      |
| RSSCloud        | Realtime push via RSS-Cloud                                                 | -      |


## Social: ActivityPub

| Title                                              | Description                                           | Status |
| -------------------------------------------------- | ----------------------------------------------------- | ------ |
| Self-Federation                                    | Connects to other Emissary servers                    | READY  |
| Secure Mode                                        | Supports Authorized Fetch from Mastodon and others    | READY  |
| Federate with [Mastodon](https://joinmastodon.org) | Send/Receive posts, replies, and likes                | READY  |
| Federate with [PixelFed](https://pixelfed.org)     | Send/Receive posts, replies, and likes                | READY  |
| Federate with [Mitra](https://mitra.social)        | Custom work to validate Mitra -flavored ActivityPub   | READY  |
| Federate with [PeerTube](https://joinpeertube.org) | Custom work to validate PeerTube-flavored ActivityPub | READY  |

## Social: Content Actors

Streams in Emissary can be their own ActivityPub actors, and can respond to external events according to pre-defined rules.

| Title               | Description                                       | Status  |
| ------------------- | ------------------------------------------------- | ------- |
| Relay Responses     | Pages can boost responses to followers            | PREVIEW |
| Relay Child Streams | Pages can boost any child pages to followers      | PREVIEW |
| Followers Only      | Limit boosts to content from followers only       | PREVIEW |
| Manage Followers    | Choose who can (and can't) follow a content actor | -       |

## Social: IndieWeb

| Title        | Description                                | Status      |
| ------------ | ------------------------------------------ | ----------- |
| IndieAuth    | rel=me tags in personal profiles           | READY       |
| IndieAuth+   | Social sign in via external services       | -           |
| MicroFormats | Default templates coded with Microformats  | READY       |
| WebMentions  | Sends/Receive WebMention updates           | PREVIEW     |
| WebMentions+ | Collect/display mentions in personal inbox | IN PROGRESS |


## Social: Next
Emerging social networking standards ([BlueSky?](https://atproto.com), Scuttlebutt? Willow?) should fit nicely into Emissary's existing architecture.  We will evaluate each to determine our future strategy.

## Looks Cool?
We need all the help we can get.   [Help us build Emissary](/developers) and be a part of the future of the Fediverse.