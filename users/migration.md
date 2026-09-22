
Emissary supports online data portability, an emerging standard on the Fediverse.  This is a quick **guide for users** to understand the account migration process.  If you're looking for a more technical description, please visit the [Account Migration for Developers](/developers-migration) page.

## Migration Requirements

Account Migration is very new, and most Fediverse servers don't support this feature yet.  As of December 2025, only Emissary servers support this feature.  You're able to move *from* an Emissary server *to* an Emissary server with just a few clicks.  We'll be updating this article over time as other kinds of servers add this important feature.

## How It Works

When you migrate servers, you're giving permission for your new server to *pull* records from your old server.  The process is quick, but can take more or less time depending on the amount of data being transferred.

### 1. Give Permission to Migrate
To start, you'll need to sign in to your new Emissary server -- *the one you're migrating to* -- then navigate to navigate to **User Settings \> Profile \> Import** to start the import process.  

This sends you to your original server where you'll need to sign in and give your permission to export your data.  When you confirm this on your original server, you'll return to your new server where you can watch the import progress.  This can run in the background, so it's okay if you need to close your browser and come back later.

### 2. Double Check Your Data
When the import finishes, you'll see a report of all the records that were imported, and any errors that were encountered along the way.  Now it the time to inspect your new profile to verify that everything was migrated correctly.  If there are any errors, you'll need to reach out to your system administrator for help.

### 3. Tell Everyone You've Moved
The last step is to announce your new location to your followers on the network.  This last step needs to happen on your original server, because the announcement messages need to be cryptographically signed with your original private keys.  

Return to the **User Settings \> Profile \> Import** page, and look for the "Continue" button near the bottom of the confirmation page.  Clicking this will return you to the "Export" page on your original server one last time.  There, you can finalize this process and send "Move" announcements to everyone in your network.  When this is done, you'll be signed out of your original Emissary server and your old account will be closed.

### 4. Following Through
Most servers on the Fediverse will understand the "Move" announcements that we send, but not all of them.  So, your original Emissary server may still receive requests for your profile and content.  While all of your content will be deleted from the original server promptly, any requests for these old records will be forwarded to your new server for a period of time.  This will help you to maintain the continuity of your identity and relationships even with friends using older software versions.


## Questions?
[Please reach out](https://mastodon.social/@benpate) if you have any questions about how this works, or if you need help migrating your account from one Emissary server to another.