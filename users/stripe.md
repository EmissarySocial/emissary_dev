
[Stripe](https://stripe.com) is a sophisticated payment platform that supports globarl payments in 195 countries around the world.  Emissary can use your Stripe account to accept individual payments, along with membership in the [Circles](/circles) that you designate for sale.

## Use Your Own Stripe Account

When you use Stripe with Emissary, you'll be using your own Stripe account.  This means that all funds you receive will go directly to you, and without your server owner being a "middle-man".  It also means that you take responsibility for everything you sell online, and liability for chargebacks, refunds, etc. 

## Stripe Connect

If you're using a large, multi-user server (such as Bandwagon.fm) your server admin has likely set up [Stripe Connect](https://stripe.com/connect), a payment platform that is simple for you to set up and collect payments.

To get started with Stripe Connect, just select it from the list of payment processors.  You'll have the option to sign in to your existing Stripe account, or to create a new one.  The "new account" onboarding process is straightforward, and will collect all of the local regulatory information necessary for you to accept online payments.

When you're done, your Stripe account will be connected to your server profile, and you'll be able to connect products and receive payments directly from your Emissary website.

## Stripe API Keys

If you're setting up your own Emissary server, the easiest way to do this is to enter your Stripe API keys (instead of using Stripe Connect).  To do this, go to Server Settings \> External \> Connections, and select "Stripe" from the list of external services.  Here, you can enter API keys that let your server make Stripe transactions on your behalf.

Emissary protects your API keys in an encrypted "vault" that is only accessible to specific parts of the application.

### Restricted Keys

For added security, Emissary uses "Restricted Keys" to sign in to your Stripe account.  These have limited permission to perform actions in your Stripe database.  You should create a specific restricted key for your Emissary database that includes ONLY these permissions:

| Resource Type | Permission   | Notes                                                              |
| ------------- | ------------ | ------------------------------------------------------------------ |
| Customers     | Read Only    |                                                                    |
| Products      | Read Only    |                                                                    |
| Checkout      | Read + Write | Required to create checkout sessions                               |
| Prices        | Read Only    |                                                                    |
| Subscriptions | Read + Write | Allows customers to cancel subscriptions themselves                |
| Webhooks      | Read + Write | Allows Emissary to receive notifications when subscriptions change |
