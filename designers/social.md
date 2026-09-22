
Apps that you make with Emissary can use all of its social features with very little effort.  Here's a few examples of social features that work today.  Help us design the social web of tomorrow.

## Multi-Network Publishing

Today, Emissary publishes streams automatically on three separate networks: 

1. [Fediverse ActivityPub](https://www.w3.org/TR/activitypub/)
2. [RSS](https://en.wikipedia.org/wiki/RSS) + [WebSub push notifications](https://indieweb.org/WebSub)
3. [IndieWeb](https://indieweb.org) + [WebMention ](https://indieweb.org/Webmention)

More protocols are on the way, with [BlueSky AT protocol](https://trello.com/c/TddJNHhs), [Scuttlebutt](https://trello.com/c/IjV9Q0lC), [Gemini](https://trello.com/c/Mhv2XdtW), and [Willow](https://trello.com/c/fqm3GA3w) on deck as potential additions.

## Publish to the Actor's Timeline

Out of the box, Emissary manages everything required to publish on the modern social web: Actors, Outboxes, Encryption Keys, and more.
As a template designer, you choose to publish any kind of Stream directly to the Actor's outbox, by adding a single command to the template's action pipeline.

#### Example Template

```json
{
	"socialRole":"Article", // define the social type
	"actions": {
		"publish": {
			"roles": ["owner", "publisher",
				"steps": [
					// save the record to the database
					{"do":"save"},
					
					// publish to the Actor's timeline
					{"do":"publish} 
				]
		}
	}
}
```

## App that Announce Their Children

Your apps can also be their own Actors, boosting all child records created within them.

```json
{
	"actor": {
		"social-role":"Application", // define the social type
		"boost-children":true, // Boost all Streams created underneath this one.
	}
}
```


## Apps that Announce Mentions

Your apps can be their own actors, boosting messages received to all of their followers.

```json
{
	"actor": {
		"social-role":"Application", // define the social type
		"boost-mentions":true, // Boost all mentions received in the inbox
		"boost-followers-only": true, // Only boost messages received from Followers
		"publish-followers": true, // Allow others to see the list of Followers
	}
}
```

## Custom ActivityPub Data

By default, Emissary populates all of the common fields you'd expect from an ActivityPub application.  But if you're making something specific, you may need to include some custom data that's outside of the norm.  Emissary uses [Rosetta Translations](https://github.com/benpate/rosetta/tree/main/translate) to let you specify any additional data you want to add into the output.  Just use add a "socialRules" element to the root of your `template.json` file with the translation you want to use.  Here's some example code that overwrites some data (like the object "type") and adds some additional links to the profile:

``` json
{
	socialRole: Article
	socialRules: [
		{target:"type", value:["Album","Article"]}
		{target:"attributedTo.id", path:"attributedTo.profileUrl"},
		{target:"attributedTo.name", path:"attributedTo.name"},
		{target:"links", forEach:"data.links", rules:[
			{target:"label", path:"key"}
			{target:"rel", value:"alternate"}
			{target:"href", path:"value"}
		]}
		{if:"{{ne \"\" .data.musicBrainzId}}", then:[
			{target:"musicBrainzId", path:"data.musicBrainzId"}
		]}
	]
}
```

## Custom Inboxes

Emissary is bundled with a default User inbox [Template](/templates) that mimics an RSS reader.  But like all other templates, system administrators have the option to install customized inboxes for users. Upcoming versions of Emissary will allow users to [select the inbox template](https://trello.com/c/N9T3RC7U) that they want to use with their profile.

## Custom Outboxes

Emissary is bundled with a default User profile/outbox [Template](/templates) that mimics other social media applications.  But like all other templates, system administrators have the option to install customized outboxes for users.  Upcoming versions of Emissary will allow users to [select the outbox template](https://trello.com/c/OYU1ETfx) that they want to use for their profiles.

## What's Next?

These social features are a placeholder for the future to come.  Please help us design a new generation of social web apps on the foundations we have laid here.