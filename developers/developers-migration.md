
Emissary supports online data portability, an emerging standard on the Fediverse. This is a technical guide for software developers to understand how Emissary implements the account migration process. If you’re looking for a straightforward description for regular people, please visit the [Account Migration for Users](/migration) page.

## W3C Data Portability Standard

Emissary's import/export tools are built on the [LOLA Portability standard](https://swicg.github.io/activitypub-data-portability/lola) as defined by the [W3C Social Web Incubator Group (SWIG)](https://github.com/swicg/activitypub-data-portability).  This is an emerging standard that is still being refined.  As such, Emissary uses a number of proposed extensions that have not been finalized, and which will be highlighted here.  As the standard evolves, Emissary will be updated to remain in sync with the W3C standard.

In this process, most of the work of moving a user's data is performed by the target server -- the target server pulls records from the source server and maps them into their new locations.. The source server is only responsible for 1) verifying the user's desire to export their data, 2) providing data in one of several formats, and 3) sending signed `Move` activities to the network at the end of the process.

Let's walk through the LOLA process step-by-step:

## 1. ActivityPub Actor Enhancements

The LOLA spec details a number of enhancements to ActivityPub Actor JSON-LD documents, which [advertise](https://swicg.github.io/activitypub-data-portability/lola#discovery) a server's ability to export data, and detail the formats available.

There are standard collections defined in the [Feature Discovery](https://swicg.github.io/activitypub-data-portability/lola#feature-discovery) section which represent profile contents as ActivityStreams objects, and are intended for broad support among Fediverse servers.  Emissary does not currently implement these, but will add them as other servers adopt the LOLA standard.

Instead, Emissary implements a number of "application-specific" collections that are allowed in the spec, and provide data in Emissary's own internal database format.  This allows us to export/import data exactly as it is within the Emissary database, providing for high-fidelity migrations that do not lose any user data.

If you're implementing LOLA data portability in your own software, [please reach out](https://mastodon.social/@benpate) and we'll coordinate testing of the standard data formats.

### IN FLUX: OAuth refresh tokens
The LOLA spec currently does not define an endpoint for OAuth refresh tokens.  This is required for OAuth servers to provide short-lived Access Tokens to clients.  Emissary is currently using the standard OAuth endpoints defined in the ActivityPub actor.  When the spec finalizes the location of OAuth endpoints, Emissary will be updated.

### IN FLUX: Additional endpoints
[This GitHub issue](https://github.com/swicg/activitypub-data-portability/issues/56) details two potential issues with the LOLA workflow, and proposes two additional endpoints that 1) allow target servers to tell the source server the URL of the user's new profile, and 2) allow source servers to tell target servers where the user should go in order to complete the `Move` migration.

While these endpoints are not strictly necessary to complete a migration, they greatly reduce the cognitive load on the user.  

Emissary will adopt any changes to this workflow that are finalized in the LOLA spec.  And when other servers implement LOLA, Emissary will include workarounds for servers that do not provide this information.

## 2. OAuth Access Token

End-users begin the migration process on the target server, by entering the URL (https://example-server.social/@username) or Fediverse Handle (@username@example-server.social) of the account they wish to migrate from.  If the user enters a handle, then their actor ID is retrieved using standard WebFinger lookup.  The target server then initiates an [OAuth 2.0](https://oauth.net) handshake to retrieve an Access Token that grants the `activitypub_data_portability` scope.  This scope grants permission for the target server to access all user records on the source server.

### IN FLUX: Using CIMD to create client\_id dynamically
[One open issue](https://github.com/swicg/activitypub-data-portability/issues/49) in the current LOLA spec is the ability for target servers to create an OAuth client\_id dynamically.  Several technologies have been proposed for this, with the most preferred solution being [Client ID Metadata Documents (CIMD)](https://client.dev).

Emissary target servers currently use CIMD to establish a client\_id on their source servers, and will update this behavior in the event that the spec changes.

## 3. Catalog Available Data

After the OAuth handshake, the user is returned to the target server which will builds an import plan.  To build its import plan, the target server scans the source server for all of the collections that are available to export.  In the Emissary UX, the user sees each collection listed on their screen, along with notes about what can and can't be imported, and a confirmation button to start importing records.

In this initial release, Emissary only works with Emissary-specific collections.  In the future, Emissary will detect data from other standard sources, too, and do its best to map those into Emissary profiles as well.

## 4. Import Individual Records

After building the import plan, Emissary walks through each collection in it to add `ImportItem`s for each record to be imported.  These records will also function as URL mappings in the "Oracle URL" to be used when forwarding requests after the migration (see Section 10 below).

When all of the collections have been cataloged, Emissary downloads each record in order. This is performed one-at-a-time by Emissary's background queue.  Using a background queue means that users are able to leave or close their web browsers if necessary without disrupting the import process.  

(Delays) Emissary's background queue manages many other operations, so imports may be delayed if the server has other, higher priority tasks to complete.  

(Delays) In addition, target servers obey [HTTP 429 "Too Many Requests"](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/429) responses, and will delay the process if requested by the source server.

### IN FLUX: `startMigration`

Before importing any records, Emissary target servers look for the `startMigration` endpoint in the source Actor's JSON-LD.  This endpoing is described above in Section 1, and [detailed in GitHub Issue #56](https://github.com/swicg/activitypub-data-portability/issues/56).

If the `startMigration` endpoint exists, Emissary sends a `POST` transaction to it, to tell the source server the location of the user's new profile along with the "Oracle URL".

## 5. Report Progress to User

While records are being imported, Emissary displays an animated progress bar with an estimate of the time required to complete the process.  Once all of the records have been imported, this page refreshes with a report of any errors that were encountered during the import.

## 6. Manual Verification Step

Once all records have been imported, the Emissary UX prompts users to manually verify their profiles.  Great care is taken to prevent and route around errors in the process, but there may be cases when bad data in a source profile cannot be mapped correctly into the target.  These broken records will be reported to users, but users will likely NOT be able to fix them independently.  System administrators with direct database access may be required to fix errors on behalf of their users.

Users see a button that confirms their data is correct and takes them to their original server to complete the process (see Section 7 below)

Users also see a button to "Cancel" the import, which deletes all of the imported records from their profile, reverting their profile to the state it was in before the user initiated the import.

## 7. Approve Migration on Source Server

Once the user is satisfied with their imported data, the final step of the migration process is to send an ActivityPub `Move` activity to other servers on the network, as documented in the [LOLA spec Section 3.3.3](https://swicg.github.io/activitypub-data-portability/lola#timing-for-copying-content-copying-follows-and-issuing-move) and in [FEP-7628](https://codeberg.org/fediverse/fep/src/branch/main/fep/7628/fep-7628.md).

To be valid, the `Move` activity must be signed by the user's private key on their original source server, so this process must take place on the source server.  LOLA does not currently specify what the workflow is to return the user to their original server, so Emissary has proposed an addition to the LOLA spec (below) to smooth out this UX for end users.

Once the user returns to their source server, Emissary presents a dialog that details the process and asks the user to confirm that they understand the irreversible step they about to take.  

### IN FLUX: `finishMigration` endpoint
As proposed in [this GitHub issue](https://github.com/swicg/activitypub-data-portability/issues/56), Emissary target servers use the `finishMigration` endpoint (if present) to deep link to the "export" page on the user's source server.  If this value is not present in the Actor document on source server, then Emissary simply forwards the user to their profile on the source server.

If this value is adopted and changed in final versions of the LOLA spec, then Emissary will adopt the new standard accordingly.  If this is dropped from the spec, then Emissary will likely publish an optional FEP to maintain this deep linkind feature.

## 8. Send `Move` Activity to Target Server

When the user confirms the finalization dialog, the source server immediately sends a `Move` activity to the target server, which is signed by the user's private key (as described above). The target server receives this message and processes it in real time (not in a background queue) so that the new status of their profile is available instantly.

At this time, the user's profile is closed and marked with a standard [`movedTo` property](https://swicg.github.io/miscellany/#movedTo).  The user is also signed out of the source website.  From this point forward, they will no longer be able to sign in to their profile on the source server.

## 9. Send `Move` Activity to Followers

After signing the user out of their original account, the Emissary source server begins a background process to send `Move` activities to each of the user's followers.  This may take some time, depending on network conditions, load on the queue itself, and the number of followers to be notified.

If there are network problems when sending `Move` activities, they will be retried a number of times until the follower's server acknowledges receipt of the activity.

In addition, all records in the user's profile are permanently deleted from the database (with exception of streams, described below)

## 10. Forward Lost Requests

The source server may still receive requests for the user's profile or content for some time after their account has closed.  This may be because of old search results, backlinks that could not be updated, or Fediverse servers that do not process the `Move` activity correctly.  For these situations, Emissary maintains header information for every user post that includes the "Oracle URL"  forwarding address provided by the target server.

### IN FLUX: Locating the Oracle URL

[This GitHub issue](https://github.com/swicg/activitypub-data-portability/issues/57) details issues with locating the Oracle URL.  

Currently, Emissary target servers send the Oracle URL to source servers using the `startMigration` endpoint described above.  The LOLA specification may move this Oracle URL to another standard place, such as a `/.well-known` URL.  In this event, Emissary will be updated to follow the spec.

### IN FLUX: Third Party Behavior
[This GitHub issue](https://github.com/swicg/activitypub-data-portability/issues/57) , we're still working out expectations on what a source server should forward, and how third-party servers should behave when looking for moved content.

Currently, Emissary will store header information for all user content for an extended period of time, so that third-party servers can still locate moved content even if they do not receive or understand the `Move` activity.  This is not required in the LOLA spec, but it is compatible with it.

## That's a Wrap

This document walks through the technical details of the LOLA data portability process, highlighting how Emissary implements it, and where Emissary deviates from the spec as it currently exists.  If you're building data migration into your application, [please reach out](https://mastodon.social/@benpate).  I'm happy to discuss the standard with you, and to help test your migration solution.