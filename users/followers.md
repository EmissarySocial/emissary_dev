
The things you post to your Emissary profile can be "followed" by others using either a Fediverse (ActivityPub) account, or an RSS reader.  ActivityPub followers are displayed in your **Profile \> Settings \> Followers**.  RSS readers don't create an active subscription, so they cannot be listed.

## Publish to Specific Followers with Circles

By default, posts are visible to everyone on the Open Web, and are visible to anyone who can view your profile.  You can restrict posts to specific groups of people using [Circles](/circles), which  provide precise access controls to the items you post.  You can put different followers into one or more circles, then choose which posts are available to which circle.

## Limiting Followers with Rules

The [rules](/rules) you add to your Emissary profile affect how posts are delivered to your followers.  Emissary supports several kinds of rules, including `Blocks` which function similar to blocks in other Fediverse servers.

When you block another profile or server, this prevents their posts from reaching your inbox, and it prevents any of your posts from being delivered to their inboxes.

Rules to not change or remove a "Follow" from your account so if you change or remove the rule, then messages will once again be delivered to affected followers.

## How Emissary Handles ActivityPub Followers

Emissary supports the standard `Follow` &rarr; `Accept` workflow for establishing a "Follower" connection.  Emissary accepts all follow requests, regardless of the requester; it does not have a mechanism for "approving" followers, as do some other Fediverse servers.

If a post is limited to specific circles, then it will be delivered similarly to a direct message, an only be sent to to specific people's inboxes.