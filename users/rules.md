
Emissary includes a sophisticated rules engine that lets you filter the messages that you allow in your inbox.  This is similar to the `Block` features of many other social servers, but gives you greater flexibility over how that "block" is handled.

To create or manage rules, go to your **Profile \> Settings \> Rules**.

## Rule Filters
Rules can match inbox messages in one of several ways.  You can make rules for specific people, entire servers, or even generalized keywords.

### Match a Person

You can filter posts from specific people by creating a rule and selecting "Match a Person" from the menu.  Then, you can enter their person's Fediverse handle (something like `@username@server.social`) to create a rule that applies only to them.

### Match a Server

You can filter all posts from a server or domain by creating a rule and selecting "Match a Website or Server" from the menu.  Then, you can enter that domain (something like `badguys.com`) to create a rule that applies to every account and piece of content on that server.

### Match Keywords

You can also filter posts that contain specific keywords or tags by selecting "Match Keywords & Tags" from the menu.  This will scan all content in a post (including tag names) and will limit posts that match the keywords you enter.

Keyword filters can potentially block lots of content from reaching your inbox, so you should only use them in limited circumstances, and with very specific keywords.  For instance, imagine filtering all posts that include the word "the" -- you would likely block all posts from reaching you.

## Rule Actions

When Emissary identifies a message that matches one of the rules you've set up, it can apply one of three actions.  Messages can either be `Labeled`, `Muted`, or `Blocked`.  

### Label

If you use a rule to `Label` incoming messages, then Emissary will apply an additional tag to all messages that match that rule.  This allows the message to still appear in your inbox but attaches new information to the message that was not provided by the sender.

### Mute

If you use a rule to `Mute` incoming messages, then Emissary will simply skip all messages that match that rule, and will not display them in your inbox.  By default, Muting a sender is a private action that is not seen by anyone else.  However, you can [publish this rule](#publishing-rules) if you want to notify others.

### Block

If you use a rule to `Block` incoming messages, then Emissary will `Mute` incoming messages that match that rule AND will prevent outbound messages from being delivered to that recipient.  By default, Blocking a sender is a private action that is not seen by anyone else.  However, you can [publish this rule](#publishing-rules) if you want to notify others.

## Publishing Rules

By default, all rules you create are private, and not visible to others.  So, if you `Mute` or `Block` an account online, they will not know that you have done this, and will not know that their messages are not reaching you.

You can choose to share rules, which then publishes your `Labels`, `Mutes`, and `Blocks` via ActivityPub.  Others will be able to follow and subscribe to your list of rules.  We strongly encouraged you to only share `Label` rules, which are the least intrusive for your followers, and that you reserve sharing `Blocks` for only the most dangerous online profiles.

## How Rules Affect Messages You Send

Rules that use `Label` and `Mute` actions do not affect your outbound messages.  So, if you have `Labeled` or `Muted` one of your followers, they will still receive your outbound messages.

`Blocks`, however, will prevent your followers from receiving posts that you make.  If you `Block` a follower, then your posts will no longer be delivered to their inboxes.  Blocks do not remove the original `Follow`, so if you change or remove this block at some point in the future then that follower will once again start receiving your posts.

This system is not perfect.  Remember, your posts are still visible on the public web, so a `Blocked` follower may still see what you've posted by going directly to your web profile.  If you want to limit access to your outbound posts, then you should consider posting only for a specific [Circle](/circles) of followers.

## Domain-Wide Rules

Domain administrators can create server-wide rules that apply to all accounts.  This may include `Blocks` for offensive accounts and servers, or for others who break the rules of that specific domain.  Domain-wide rules have all of the same options that user-generated rules do, and can be created in the domain admin tool, under the **Rules** section.