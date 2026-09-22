Follow this walkthrough to set up a demonstration Emissary server using Docker. Visit the  [configuration]() for more complete instructions.

<iframe title="Quickstart with Docker" class="width-100-percent aspect-16-9 margin-bottom"  src="https://clip.place/videos/embed/ioe8X75QK6RH5nY3kxuxro" frameborder="0" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups allow-forms"></iframe>

[Docker](https://www.docker.com) is a common way to wrap a server and all of its dependencies into a single "container" configuration.  Containers can be shipped to many different cloud hosting environments.  If you use Docker, it's easy to get Emissary up and running  in just a few easy steps.

Emissary ships with two Docker configurations: a demonstration container to start testing quickly, and a production container that you can use in production.

## Demo Container

Make sure you have Docker installed, and download the [Emissary source code](https://github.com/EmissarySocial/emissary) to any directory on your machine.  We're going to use [docker-compose](https://docs.docker.com/compose/) to create a new container on your machine.

1. Open a terminal to the directory where you've downloaded Emissary
2. Enter the command `docker-compose up` to build the container.

This container will include all of the dependencies that Emissary needs, and will configure a single "demo" domain for you to explore.

3. Wait for the container to build. When the server is online, you'll see the Emissary banner in your terminal.
4. Open your web browser to `http://localhost:8080`.  You'll see a sign-in prompt before you can continue
5. Sign in to the demonstration server with username: `demo` and password: `demo`
7. You're in! Choose the theme you want to use for your demo site, and start exploring what Emissary can do.

## Production Container

Production installations have many specific requirements that depend on the rest of your hosting environment.  Emissary is very flexible and will work in a wide range of cloud hosting and bare metal installations. So, there is no "one size fits all" Docker container for production servers.

However, Emissary *does* include a production compose file that you can customize to fit your needs.

```
% docker-compose -f docker-compose-production.yml up
```

This production container requires that you define environment secrets in your docker configuration for the `EMISSARY_CONFIG` environment variable.  This tells emissary where to find its configuration database.  For production docker environments, you are strongly encouraged to use a mongodb database for your Emissary configuration, and to NOT use a configuration file.  Please visit the [Configuring Emissary](https://emissary.dev/configuring) page for more details on Emissary's many configuration options.