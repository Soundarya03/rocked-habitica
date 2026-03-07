# Rock-ed Habitica for self-hosting

This project builds a [rock](https://documentation.ubuntu.com/rockcraft/stable/explanation/rocks/#explanation-rocks)—a new-age, minimal container image—of the Habitica app for self-hosting, using the open source tools [rockcraft](https://documentation.ubuntu.com/rockcraft/stable/) and [chisel](https://documentation.ubuntu.com/chisel/latest/).

This builds upon [awinterstein/habitica](https://github.com/awinterstein/habitica), which consists of adaptations for self-hosting Habitica and is a fork of the upstream Habitica app itself.
In place of building a regular Docker image, however, this builds a **chiseled rock, which minimizes the size (and by extension attack surface) of the container image by over 45%**.

The license from the upstream projects applies here too.

## How to use

This is a step-by-step guide to build the habitica rock, i.e, the lightweight OCI-compliant container image,
and compose it alongside other services it may need such as a mongo database, and have the app up and running on your local system.

1. Install pre-requisites

Refer [this](https://documentation.ubuntu.com/rockcraft/stable/tutorial/hello-world/#setup-your-environment) quick tutorial's "Setup your environment" section to install rockcraft and the tools you will need.
You can follow along with the rest of the below steps on the Ubuntu VM set up in the tutorial.

2. Build the rock

Clone this repository, `cd` into the root of this project, and simply run

```commandline
rockcraft pack
```

This should provide you with an OCI archive (a `.rock` file). Congratulations! Your OCI-compliant container image is now available.

3. Import the rock into Docker to run locally

The great part about the rock being an [OCI-compliant](https://opencontainers.org/) container image is that it can be used alongside other OCI-compliant tools.
To run our container locally, let's execute the following command to import our new rock into Docker:
```commandline
sudo rockcraft.skopeo --insecure-policy copy oci-archive:habitica-app_2.0_amd64.rock docker-daemon:habitica-app-rock:chiseled
```
Change the names as required.

4. Use Docker compose to orchestrate your services
You can find a `compose-rock.yaml` file in this project. Run the following command to bring up your full-stack app:
```commandline
docker compose -f compose-rock.yaml up
```
You've now got a full-working Habitica instance accessible at localhost:3000!
You can customize this docker compose file as needed.

## Readme of awinterstein/habitica

Adaptions and infrastructure to facilitate self-hosting of the habit-building program [Habitica](https://habitica.com). It is based on the source code and assets of the [Habitica Repository](https://github.com/HabitRPG/habitica), hence the [LICENSE](https://github.com/HabitRPG/habitica/blob/develop/LICENSE) from there applies here and to the adaptions in this repository as well.

![Screenshot of the Habitica Web Client](website/client/public/static/presskit/Samples/Website/Market.png)

For each release in the Habitica upstream repository, the self-hosting adaptions are automatically applied by rebasing the `self-host` branch onto the last release commit. The Docker images for server and client are built then and pushed to Docker Hub as [awinterstein/habitica-server](https://hub.docker.com/r/awinterstein/habitica-server) and [awinterstein/habitica-client](https://hub.docker.com/r/awinterstein/habitica-client).

### Improvements for Self-Hosting

The following noteworthy changes were applied to the Habitica source code:

- Dockerfile and Github Workflow to create the production containers for hosting
- every user automatically gets a subscription on registration that never needs to be renewed
- first registered user automatically gets admin rights
- locations for buying gems with money were replaced with options to buy with gold (e.g., the quick access in the header)
- group plans can be created without payment
- emails are sent directly via a configured SMTP server instead of using the Mailchimp (Mandrill) web-service
- settings and links (e.g., in the footer) that do not make sense for a self-hosted site were removed
- registrations can be restricted to only invited users via a configuration parameter
- analytics and payment scripts are not loaded

### Limitations

The following things do not work (yet):
- third-party access and scripts are not thoroughly disabled, so there might still be some scripts loaded

_(This is a shortened version. For the full Readme of the upstream awinterstein/habtica, please check [here](https://github.com/awinterstein/habitica).)_

## Readme of the Upstream Habitica Repository

![Build Status](https://github.com/HabitRPG/habitica/workflows/Test/badge.svg)

[Habitica](https://habitica.com) is an open-source habit-building program that treats your life like a role-playing game. Level up as you succeed, lose HP as you fail, and earn Gold to buy weapons and armor!

**Want to contribute code to Habitica?** We're always looking for assistance on any issues in our repo with the "Help Wanted" label. The wiki pages below and the additional linked pages will tell you how to start contributing code and where you can seek further help or ask questions:
* [Guidance for Blacksmiths](https://habitica.fandom.com/wiki/Guidance_for_Blacksmiths) - an introduction to the technologies used and how the software is organized.
* [Setting up Habitica Locally](https://github.com/HabitRPG/habitica/wiki/Setting-Up-Habitica-for-Local-Development) - how to set up a local install of Habitica for development and testing.

**Interested in contributing to Habitica’s mobile apps?** Visit the links below for our mobile repositories.
* **Android:** https://github.com/HabitRPG/habitica-android
* **iOS:** https://github.com/HabitRPG/habitica-ios

Habitica's code is licensed as described at https://github.com/HabitRPG/habitica/blob/develop/LICENSE

**Found a bug?** Please report it to [admin email](mailto:admin@habitica.com) rather than create an issue (an admin will advise you if a new issue is necessary; usually it is not).

**Creating a third-party tool?** Please review our [API Usage Guidelines](https://github.com/HabitRPG/habitica/wiki/API-Usage-Guidelines) to ensure that your tool is compliant and maintains the best experience for Habitica players.

**Have any questions about Habitica or contributing?** See the links in the [Habitica](https://habitica.com) website's Help menu. There’s FAQ’s, guides, and the option to reach out to us with any further questions!
