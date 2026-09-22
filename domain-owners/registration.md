
User Registration in Emissary uses customizable templates that are similar to Themes, Templates, and Widgets.

Currently, two registration apps are built in to Emissary, with more options on the way.  Additional registration apps can be installed in your Emissary database along with other packages.

**[Free Registration](/registration-free)** <br>
A simple, free signup form that lets new users register with their name and email address

**[Paid Registration via Stripe](/registration-stripe)** <br>
A customizable registration process that uses Stripe Payments to charge users for one or more membership tiers.

**[Registration via WebHooks](/registration-webhooks)** <br>
A customizable registration process that uses Webhooks to create new users


## Selecting a Registration App

To set up new user registrations on your Emissary webiste, go to Settings \> Users and click on the section labeled "New User Signups".  This will display a dialog box where you can choose which of the installed registration apps you want to use and enter any additional settings required by the individual app.  Each registration app will require it's own specific configuration, so visit the help documentation for the specific app you're using.

Once you've configured your user registration app, a link will appear on your signup page where new users can register.

## Welcome Emails

Emissary employs a number of techniques to prevent fraudulent signups.  One of these is that user accounts must be confirmed via email before they are created.  So, once a new user registration is received, an email is sent to the user's email address to confirm their identity.  The email includes a link with their encoded signup information.  Their signup will be finalized once they click the link and return to the Emissary server.