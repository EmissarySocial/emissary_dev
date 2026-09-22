
Emissary includes a registration template for connecting to external services.  This lets you send a WebHook notification to your emissary server to register new users programmatically.

**To Set Up This Option:**
1. Go to **Server \> People \> Users** on your Emissary server
2. Click on the **New User Signups** option in the top left corner of the page.  
3. This will open a dialog box where you can choose the registration app you want to use.  
4. Choose **External Service** to begin.

You'll need to fill out a few pieces of information, that are labeled and described on the page.

| Value             | Description                                                                                                                                                                                                        |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Signup URL**    | The URL on your external service where users can sign up.  This is displayed on the login page to prompt new users to sign up.                                                                                     |
| **Portal URL**    | The URL on your external service where users can manage their accounts. This should let them update their usernames, passwords, email address, or cancel their account.                                            |
| **Shared Secret** | This must be a long, un-guessable value that is used to validate transactions coming from your server.  It must only be stored and used on your external servers, and must not be displayed anywhere on your site. |

## Sending WebHooks to Emissary

Once Emissary is ready to accept registrations from your external service, you'll need to set up your external service to actually post those transactions to Emissary.  Here is the information you'll need to send from your external service.

### WebHook Details

|             |                                       |
| ----------- | ------------------------------------- |
| HTTP Method | `POST`                                |
| URL         | `https://<YOUR_SERVER_NAME>/register` |
| Encoding    | `application/x-www-form-urlencoded`   |

### Form Fields

| Field            | Description                                                                                                                                      | Visibility |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- |
| `displayName`    | Publicly displayed name for this user                                                                                                            | PUBLIC     |
| `username`       | Unique username that identifies the user                                                                                                         | PUBLIC     |
| `emailAddress`   | Email address where the user's invite email will be sent                                                                                         | PRIVATE    |
| `inboxTemplate`  | [Template ID](https://emissary.dev/templates) to use for the User's inbox                                                                        | PRIVATE    |
| `outboxTemplate` | [Template ID](https://emissary.dev/templates) to use for the User's outbox (profile page)                                                        | PRIVATE    |
| `addGroups`      | Comma separated list of Group IDs or tokens to add the user to when they complete their registration.                                            | PRIVATE    |
| `secret`         | The shared secret that validates the transaction. To maintain security of your registration form, this must not be shared or displayed anywhere. | PRIVATE    |
