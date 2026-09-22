
Emissary is designed to be as simple as possible, with very few external dependencies.  The main application server is written in [Go](https://go.dev), which uses a [MongoDB](https://mongodb.org) ([or compatible](https://www.ferretdb.io)) database.  That's it.  

The web app uses two new javascript libraries [Htmx](https://htmx.org) and [Hyperscript](https://hyperscript.org) for a fast and fluid user experience.  Understanding these tools is helpful -- but not essential -- because they're far simpler than other front-end Javascript and very easy to learn.

Everything else is baked into a single executable that compiles and runs on everything from a single VPS to a cloud-based Kubernetes cluster and beyond.  Many of Emissary's core components have been broken out into their own open source libraries -- like [Rosetta](https://github.com/benpate/rosetta) data manipulation, and [Hannibal](https://github.com/benpate/hannibal) ActivityPub libraries -- so if you can take advantage of Emissary even if you're building something different.

## Documentation

Ready to dig in deeper?  Check out the [GitHub repository](https://github.com/EmissarySocial/emissary), or read through the [GoDoc Documentation]()