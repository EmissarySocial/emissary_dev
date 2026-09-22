
<iframe title="New User Registration with Stripe" class="width-100-percent aspect-16-9 margin-bottom" style="border:solid 1px black;" src="https://kumi.tube/videos/embed/68dee507-92d4-4ae2-b76c-b31a4bc5a063" frameborder="0" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups"></iframe>

[Stripe](https://stripe.com) is a secure and reliable payment processor that integrates directly with Emissary.  Emissary includes a customizable registration workflow that uses Stripe Subscriptions to manage users' accounts on your Emissary server.

You might set up your Emissary database to work with Stripe in many ways, so this process is not automated. There are many manual steps, but fortunately, they are all pretty easy to do.

## 1. Emissary - Set Up Groups

Emissary uses "groups" to control users' access to your domain.  Your first step is to set up one or more groups that will define users' various access levels.  In your Emissary database, go to **Settings \> Groups** to add the groups you'll need for user access control.

When you create groups to integrate with Stripe, it's best to give each group a unique `Token` value that is easily readable.  This is easier to set up and troubleshoot once we connect these groups to Stripe (in step 3.1 below)

## 2. Stripe - Create an Account

To open a Stripe account, you'll need the same basic information required to open a bank or other financial account. Go to ￼stripe.com￼(https://stripe.com) to get started. Stripe may need a few days to verify your financial information before your account is activated, but you should be able to start the rest of the configuration right away.

## 3. Stripe - Create Products with Subscription Pricing

Once you can access your Stripe dashboard, click the [Product Catalog](https://dashboard.stripe.com/test/products) section to create one or more "products." These will represent the membership tiers available to your users.

Each product in Stripe uses one or more pricing methods, which can be set up as subscriptions that receive recurring payments.  For example, you can make monthly and yearly prices for the same "product."

### 3.1. Stripe - Configure Product MetaData

Stripe allows you to add custom "metadata" to each product. Emissary uses the values below to control users' access. Without them, your Stripe integration will not work correctly.  

To enter product metadata, select a product from the Stripe Product Catalog and click the "Edit Metadata" button about halfway down the page.  There are three values that you can enter:

| Field            | Description                                                                                                                                                                                                                                               |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| add\\\_groups    | A comma-separated list of group `ID`s or `Token`s to add to a User's profile when they purchase this product/subscription. If this is blank, no groups will be added to new users' accounts.                                                              |
| remove\\\_groups | A comma-separated list of group `ID`s or `Token`s to remove from a User's profile when they purchase this product/subscription. This may be used if several overlapping pricing models give access to different service levels in your Emissary database. |
| set\\\_public    | If set to "true," then users are marked as "public" when their subscriptions activate and "hidden" when their subscriptions expire.                                                                                                                       |

## 4. Stripe - Create a Pricing Table

Once you have set up your subscription options, go to [Product Catalog \> Pricing Tables](https://dashboard.stripe.com/pricing-tables) to create a pricing table.  This is Stripe's "no code" solution for configuring a simple checkout table that we'll include on your Emissary website.

Here, you can choose the various products and pricing options that you want to feature on the pricing table.  When you're done, save your changes and return to your Emissary database to set up the links to Stripe.

## 5. Stripe - Create a Webhook
"Webhooks" are a way for Stripe to send messages to Emissary, such as when a subscription begins or ends. Emissary needs this information to add/remove users and their groups.

In your Stripe account, go to [Developers \> Webhooks](https://dashboard.stripe.com/webhooks) and click the "add endpoint" button near the top of the page.  You'll get a page where you can name and configure a new Webhook.

| Field        | Value                                    | Description                                                                                             |
| ------------ | ---------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Endpoint URL | "https://\\[your-domain-name\\]/.stripe" | Replace "your-domain-name" with your actual Emissary domain, and make sure the URL ends with `/.stripe` |
| Description  | "Emissary Webhook"                       | This description is just for you, but make sure it's clear that this data is going to Emissary          |
| Listen to    | "events on your account"                 | Leave this default in place.  Emissary does not need access to any "Connected Accounts" in Stripe.      |

In the last part of this setup page, you must choose the events that Stripe will send to Emissary.  Stripe generates LOTS of events; Emissary doesn't need most of that data.  Click the "select events" button, then enter "customer.subscription" in the search box to display the eight or so events related to subscriptions.  Check all these events, then click "Add events" at the bottom of the page.

Last, click the "Add endpoint" button after the page refreshes, and your new Webhook will be ready to go.

You're done using your Stripe account for now, but leave this tab open because you'll need to copy/paste some values from it in the next step.

## 6. Stripe - Create a Restricted API Key

Stripe gives you lots of control over the data that applications can access and modify.  For security reasons, we want to give Emissary *as few privileges as possible*.  So, Emissary does not use the general-purpose "API Key" that Stripe created automatically.  Instead, we need to create a `Restricted Key` that has very limited permissions.

In your Stripe account, go to the [Developers \> API Keys](https://dashboard.stripe.com/apikeys) section and click the "Create Restricted Key" button near the bottom of the page.  This will give you a page listing the many things Stripe can do.  We only want to choose a couple of these values so that we know for sure that Emissary isn't able to read or change any data that it's not supposed to.  Select the following values, then save your Restricted Key.

| Resource | Permission | Reason |
| Customers | READ | Emissary reads details about your customers to pre-populate their name and email address when they sign up.  |
| Products | READ | Emissary reads details about your products to retrieve the product metadata (from 3.1) that drives users' access privileges  |

Set the permissions for the resources above to "READ".  Leave all other resources as "NONE", and leave ALL values in the "Connect Permissions" as "NONE" as well.  Emissary only reads these values to process your customers' subscriptions.  Emissary NEVER writes directly to your Stripe account.

## 7. Emissary - Configure Registration

Now that you've set up Stripe's Products and Pricing Tables, we must tell Emissary how to use them.  

Sign in to your Emissary database, go to the **Settings \> Users** section, and click the "New User Signups" button near the top of the page.  You'll get a dialog that lets you choose the registration app you want to use.  In the first drop-down, select "Register with Stripe", then the dialog will update with a list of fields that Emissary needs to continue.  All of these fields are required.

### 7.1. Publishable Key
The publishable key is a public identifier that is safe to share online.  It points Stripe to your account and the resources you've published within it.

In your Stripe dashboard, navigate to [Developers \> API Keys](https://dashboard.stripe.com/apikeys) section, copy the "publishable key", and paste it into the correct field in Emissary.  

Publishable keys begin with `pk_prod_` followed by 24 hexadecimal numbers.

### 7.2. Restricted Key
The "restricted key" is the value we set up in section 6, which gives Emissary privileges to read Customers and Products from your Stripe account.  

In your Stripe dashboard, navigate to [Developers \> API Keys](https://dashboard.stripe.com/apikeys) section, copy the "restricted key" that you created, and paste it into the correct field in Emissary.  

Restricted keys begin with `rk_prod_` followed by a long string of hexadecimal numbers.

### 7.3. Webhook Secret
The webhook secret allows Emissary to verify that incoming messages were sent by Stripe and not spoofed by a bad actor.  Webhooks from Stripe will be signed with this value, so Emissary needs it to validate the signatures.  

In your Stripe dashboard, navigate to [Developers \> Webhooks]([https://dashboard.stripe.com/webhooks]) and select the Webhook we created in step 5.  At the top of the page, there's a header labeled "Signing secret".  Click on the "Reveal" link in this section, copy the value, and paste it into the correct field in Emissary.

Webhook secrets begin with `whsec_` followed by 32 hexadecimal numbers.

### 7.4. Pricing Table Identifier
The pricing table identifier is the ID number of the pricing table you created in Step 4.  It displays your pricing table when new users want to register.

In your Stripe account, navigate to [Product Catalog \> Pricing tables]([https://dashboard.stripe.com/pricing-tables]) and select the Pricing Table you created.  At the top of the page is some sample HTML code that embeds this table into a web page.  We don't need all of this code, only the pricing table identifier, which is embedded in the code here:

\<stripe-pricing-table pricing-table-id="COPY-THIS-PART-HERE"...

Copy this value and paste it into the correct field in Emissary.

Pricing table identifiers begin with `prctbl_` followed by 24 hexadecimal digits.

### 7.5 Customer Portal URL
The customer portal URL is a website where users can manage their Stripe subscriptions directly. On this page, they can upgrade, downgrade, or cancel their existing memberships.

In your Stripe account, navigate to [Product Catalog \> Pricing tables]([https://dashboard.stripe.com/pricing-tables]) and select the Pricing Table you created.  You'll find the "Customer Portal" section at the bottom of the page, where you can copy the web address Stripe provides.

Copy this value and paste it into the correct field in Emissary.

### 7.6 You're All Clear, Kid.  Save Your Registration Setup
Emissary tries to validate these values as best it can, and it won't let you save the Stripe registration settings if anything is obviously malformed.  If you're having trouble, verify that you didn't accidentally copy an extra "space" character before or after configuration values.

Once your configuration is correct, click the "Save Changes" button, and your registration app will be live.

## 8. All Done.  Test Your Setup
To test, sign out of your administrator account, then click the "Sign In" link in the top right corner of the navigation bar.  The "Sign In" page should now include a link near the bottom that prompts you to Register.  You can click this link to walk through the registration process independently.  Congratulations!