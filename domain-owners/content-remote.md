
A **Remote Article** is a page on your Emissary site whose text lives somewhere else, such as in a git repository or on another static website.  Remote articles can be either plain HTML code, or Markdown.  Emissary copies the file into your page and keeps it up to date, so you can write in your own editor or version control system, and they'll always be up to date on your Emissary website.

## Creating a Remote Article

You can create remote articles anywhere on your site that where a regular "Article" type will fit.  Just click the "Add a Page" button and look in the "Special" tab.

You'll need to enter the remote URL that Emissary should use.  Any HTTP-accessible website will work, but *you must point Emissary at the raw HTML or markdown content*.  For example, you can pull content from GitHub using links that look like: `https://raw.githubusercontent.com/you/your-repo/refs/heads/main/docs/guide.md`

Files ending in `.md` are read as Markdown, and files ending in `.html` as HTML. The address must be public; Emissary cannot read private files.

## Article Metadata

By default, the meta data for each page (title, summary, token, and rank) are set when you create the page on your Emissary site.  However, you can update these values remotely by adding front-matter to your Markdown file.

```markdown
---
title: Getting Started
summary: Everything you need for your first day.
slug: getting-started
rank: 10
---
```

## Images, Video, and Other Attachments

Youc an embed images, videos, audio, and PDFs in your remote pages as well.  To do this, just create a folder named `attachments` in the same directory as the source file, then link to them the usual way:

```markdown
![A diagram of the system](attachments/diagram.png)

[Download the manual](attachments/manual.pdf)

![Watch the walkthrough](attachments/walkthrough.mp4)
```

When Emissary syncs your page, it copies each file into your site and maps the filenames into Emissary's attachments, so your page won't depend on the repository to show them. A video or audio file linked like an image becomes a player.

## Keeping Pages Up to Date

You can set up automatic updates by configuring a `WebHook` on the remote system.  Most content managers and source control websites can do this.

Just copy the **Webhook URL** from the settings screen. On GitHub, go to your repository's **Settings \> Webhooks \> Add webhook**, paste it into **Payload URL**, and choose **Just the push event**. Leave the secret empty. Other forges have a similar setting.

The **Webhook Token** determines the unique URL that updates this specific page on your website. To set up multiple pages on your website to pull from the same remote source, just choose the same Webhook token.  All pages that use the same webhook token will be synchronized whenever that unique URL is called.