---
title: Configuring Build Notifications
layout: en
permalink: notifications/
---

<div id="toc"></div>

## Notifications

Travis CI can notify you about your build results through email, IRC and/or webhooks.

By default it will send emails to

* the commit author and committer
* the owner of the repository (for normal repositories)
* *all* public members of the organization owning the repository

And it will by default send emails when, on the given branch:

* a build was just broken or still is broken
* a previously broken build was just fixed

You can change this behaviour using the following options:

## Email notifications

You can specify recipients that will be notified about build results like so:

    notifications:
      email:
        - one@example.com
        - other@example.com

And you can entirely turn off email notifications:

    notifications:
      email: false

Also, you can specify when you want to get notified:

    notifications:
      email:
        recipients:
          - one@example.com
          - other@example.com
        on_success: [always|never|change] # default: change
        on_failure: [always|never|change] # default: always

`always` and `never` obviously mean that you want email notifications to be sent always or never. `change` means that you will get them when the build status changes on the given branch.

## IRC notification

You can also specify notifications sent to an IRC channel:

    notifications:
      irc: "irc.freenode.org#travis"

Or multiple channels:

    notifications:
      irc:
        - "irc.freenode.org#travis"
        - "irc.freenode.org#some-other-channel"

Just as with other notification types you can specify when IRC notifications will be sent:

    notifications:
      irc:
        channels:
          - "irc.freenode.org#travis"
          - "irc.freenode.org#some-other-channel"
        on_success: [always|never|change] # default: always
        on_failure: [always|never|change] # default: always

You also have the possibility to customize the message that will be sent to the channel(s) with a template:

    notifications:
      irc:
        channels:
          - "irc.freenode.org#travis"
          - "irc.freenode.org#some-other-channel"
        template:
          - "%{repository} (%{commit}) : %{message} %{foo} "
          - "Build details: %{build_url}"

You can interpolate the following variables:

* *repository*: your GitHub repo URL.
* *build_number*: build number.
* *branch*: branch build name.
* *commit*: shorten commit SHA
* *author*: commit author name.
* *message*: travis message to the build.
* *compare_url*: commit change view URL.
* *build_url*: URL of the build detail.

If you want the bot to use notices instead of regular messages the `use_notice` flag can be used:

    notifications:
      irc:
        channels:
          - "irc.freenode.org#travis"
          - "irc.freenode.org#some-other-channel"
        on_success: [always|never|change] # default: always
        on_failure: [always|never|change] # default: always
        use_notice: true

and if you want the bot to not join before the messages are sent, and part afterwards, use the `skip_join` flag:

    notifications:
      irc:
        channels:
          - "irc.freenode.org#travis"
          - "irc.freenode.org#some-other-channel"
        on_success: [always|never|change] # default: always
        on_failure: [always|never|change] # default: always
        use_notice: true
        skip_join: true

If you enable `skip_join`, remember to remove the `NO_EXTERNAL_MSGS` flag (n) on the IRC channel(s) the bot notifies.

## Campfire notification

Notifications can also be sent to Campfire chat rooms, using the following format:

    notifications:
      campfire: [subdomain]:[api token]@[room id]


* *subdomain*: is your campfire subdomain (ie 'your-subdomain' if you visit 'https://your-subdomain.campfirenow.com')
* *api token*: is the token of the user you want to use to post the notifications.
* *room id*: this is the room id not the name.

## Flowdock notification

Notifications can be sent to your Flowdock Team Inbox using the following format:

    notifications:
      flowdock: [api token]


* *api token*: is your API Token for the Team Inbox you wish to notify. You may pass multiple tokens as a comma separated string or an array.

## HipChat notification

Notifications can be sent to your HipChat chat rooms using the following format:

    notifications:
      hipchat: [api token]@[room name]


* *api token*: is the token of the user you want to use to post the notifications.
* *room name*: is the name of the room you want to notify.

## Webhook notification

You can define webhooks to be notified about build results the same way:

    notifications:
      webhooks: http://your-domain.com/notifications

Or multiple channels:

    notifications:
      webhooks:
        - http://your-domain.com/notifications
        - http://another-domain.com/notifications

Just as with other notification types you can specify when webhook payloads will be sent:

    notifications:
      webhooks:
        urls:
          - http://hooks.mydomain.com/travisci
          - http://hooks.mydomain.com/events
        on_success: [always|never|change] # default: always
        on_failure: [always|never|change] # default: always
        on_start: [true|false] # default: false

Here is an example payload of what will be `POST`ed to your webhook URLs: [gist.github.com/1225015](https://gist.github.com/1225015)

### Authorization
When Travis makes the POST request, a header named 'Authorization' is included.  It's value is the SHA2 hash of your
GitHub username, the name of the repository, and your Travis token.  In python,

    from hashlib import sha256
    sha256('username/repository' + TRAVIS_TOKEN).hexdigest()

Use this to ensure Travis is the one making requests to your webhook.
