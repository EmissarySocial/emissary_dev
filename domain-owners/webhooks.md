
Emissary is created to work well in a digital ecosystem of many services and applications.  One way it does this is by supporting [WebHooks](https://en.wikipedia.org/wiki/Webhook) as a way to notify external services of key events within your website.  

For example, you can configure WebHooks to notify an external email server every time a new signs up, or whenever a new piece of content is published by you or one of your users. 

## Setting Up WebHooks

To manage WebHooks, sign in to your website as an administrator, then go to the "Server" admin section, and click on "WebHooks" in the menu bar.

When you create a WebHook, you can select one or more system events that will trigger the WebHook.  And when a WebHook is triggered, Emissary will send an HTTP `POST` request to the URL that you have configured.

## WebHook Types

| Event                   | Description                                            |
| ----------------------- | ------------------------------------------------------ |
| `stream:create`         | Occurs when a Stream is first created                  |
| `stream:delete`         | Occurs when a Stream is deleted                        |
| `stream:publish`        | Occurs when a Stream is published                      |
| `stream:publish:undo`   | Occurs when a Stream is removed from publication       |
| `stream:syndicate`      | Occurs when a Stream is published to external services |
| `stream:syndicate:undo` | Occurs when a Stream is removed from external services |
| `stream:update`         | Occurs when a Stream is updated                        |
| `user:create`           | Occurs when a User is first created                    |
| `user:delete`           | Occurs when a User is deleted                          |
| `user:update`           | Occurs when a User is updated                          |

## WebHook Resources
* https://en.wikipedia.org/wiki/Webhook - WebHooks definition by Wikipedia
* http://www.webhooks.org - Overview of WebHooks, discussions, and standard conventions
* https://webhook.site - Resource for testing WebHooks
* https://zapier.com - Commercial integration platform that helps to receive, map, and retransmit WebHook data