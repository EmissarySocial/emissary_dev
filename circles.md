
"Circles" give you control over who can see the posts and content that you publish online.  When you create something new, you can choose to make it "public" so that everyone can see it, or to limit it to one or more circles.  For instance, you may want to send a post only to a specific group of friends, or neighbors.  You can also make circles for paid subscription tiers, and charge for access to your activity streams.

**Important:** Different Emissary apps can implement circles differently, so that specific way you use it may change from app to app.

## Creating New Circles

You can create circles by going to your **Profile \> Settings \> Circles** page. There's no limit on the number of circles you can create.

### Visible vs. Hidden Circles

When you create a circle, you can mark it as "visible" to its members.  This means that the circle will show up on that person's guest profile page along with content that is linked to that circle.  

If you don't make a circle visible, then it will not show up on a person's guest profile page.  Hidden circles will still affect the content that it's members can access, but the circle itself will not be shown to them.

### Featured Circles

When you create a circle, you can mark it as "featured", which means that it will be displayed as a signup option on your personal profile page.  "Featured" circles are an easy way to show subscription options to people who visit your profile page.

## Assigning Circle Memberships Manually

When you create a new circle, you can add people manually using either their fediverse handle, fediverse URL, or email address.  Just enter either of these identifiers and that person will be added to the circle.

**Fediverse Handle** `@john@connor.social`  -- a person's username on their Fediverse server, and always begins with and "@" sign. People can get Fediverse handles from any Fediverse server. [Learn more about the Fediverse](https://fediverse.info)

**URL** `https://connor.social/@john` -- a person's profile website on the Fediverse, which usually looks similar to their handle, but always begins with "https://" 

**Email Address**  `john@connor.com` -- Email addresses are a ubiquitous form of communication from the 20th Century.

### Signing In for Private Content

Once you've done this, your friends, neighbors, and co-workers can access the private posts to that circle by visiting your profile and entering the email or Fediverse handle that you set up.  Emissary will send a message to their email or Fediverse address, containing a link and a secret code to sign them in.  Then, all of the posts reserved for their circle(s) will appear.

## Selling Circle Memberships

Circles also connect with online payments in Emissary so that you can sell access to a circle.  This works great for "members only" feeds that contain exclusive content for paying members.

Before you begin, you'll need to connect at least one merchant account to your Emissary profile. Learn more about this in the [online payments section](https://emissary.dev/payments).  Circles work with both one-time payments, or with recurring subscriptions.  So, you can 

**Important** You'll link *your own merchant account* to Emissary, so you are always in direct control of what you charge and how you get paid -- not the server that hosts your profile.

### Linking Circles to Products

Once you have set up a merchant account(s), add/edit a circle and select the "Products" tab.  This will list all products available from your merchant account.  Check off one ore more products to link them to this circle.  

If the product is a "one time" payment, then people who purchase this product through your Emissary website will be added to this circle permanently.  Similarly, if the product is a recurring, or "subscription" product, then people who purchase it through Emissary will be added to this circle *for as long as their subscription is active*.  Subscriptions and memberships are managed by your merchant account provider, but changes are synchronized with Emissary, so any changes you make in your merchant account dashboard will be reflected in Emissary.

**Example:** 
John posts three different kinds of articles on his website: "free" articles are public for everyone, limited "gold" articles are for supporters who pay $1/month, and exclusive  "platinum" articles for those who pay $10/month. All of these charges will be processed by John's own Stripe merchant account.

To charge his paid members, John should make circles for each of the two paid tiers -- one for "gold" and one for "platinum".  Then in Stripe, he can set up subscription plans and pricing for each of these groups.  It's possible to link each circle to more than one Stripe product, so John might link several prices to the "platinum" group -- for instance, charging a higher rate for monthly billing, then offering a discount for annual billing.  Each of these would be a separate subscription plan in Stripe, but both types of subscriptions would be linked to John's "platinum" circle in Emissary.

Finally, "free" articles don't require any payment, so John can just mark those as "public" when he posts them.

## Mixed Circles

Circles can contain both kinds of memberships -- both people who have paid for access, and people who are granted access manually by you.  This means that you can charge most people for access to a circle, while still granting separate "free" membership in that same circle to others.
