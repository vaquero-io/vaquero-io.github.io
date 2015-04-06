---
layout: page
title: Getting Started
permalink: /docs/getting_started.html
---

The no-nonsense guide to getting started with Vaquero.

## Installation
Vaquero is distributed as a ruby gem. Installation is as simple as:

    $ gem install vaquero_io

## [Plugins](plugins.html)
Vaquero requires a plugin to be installed in order to actually do anything. Right now, there's only a single plugin in development, `vaquero-plugin-vcoworkflow`, but development is also underway for a straight vSphere plugin as well.

See the [list of available plugins](plugins.html).

To install a plugin:

    $ vaquero plugin install <URL>

For example:

    $ vaquero plugin install https://github.com/vaquero-io/vaquero-plugin-vcoworkflow.git

## Platform definition

Vaquero operates on the basis of a "Platform Definition". This is a set of YAML files which outlines the overall structure of your application. If your'e starting from scratch, you can create a new one:

    $ vaquero new -p vaquero-plugin-vcoworkflow

This will create a set of files:

    .
    ├── infrastructure
    │   └── compute.yml
    └── platform.yml

The `platform.yml` file is where you outline the basic structure of your application.

    platform:
      provider: vaquero-plugin-vcoworkflow
      plugin_version: 0.1.0
      product: myplatform
      environments:
        - qa1
        - stage1
        - prod1
      nodename:
        - environment
        - '-'
        - component
        - '-'
        - instance
      pools:
        defaultpool: &defaultpool
          vco_url: https://vco.example.com:8281
          workflow_name: 'Request Component'
          workflow_id:
          reservation_policy: nonprod
          execute_on_behalf_of: svc_accout@example.com
          compute: small
          count: 2
          run_list:
            - role[loc_uswest]
            - role[base]
          component_role:
            - role[#]
      components:
        webserver:
          <<: *defaultpool

## Environments

Once the platform definition is written, you need to create environment definitions for each environment you need to provision (dev, stage, production). In the simplest form, you simply need to include the components of your platform which need to exist in the environment. You can also override parameters in the environment if they need to be different.

    qa1:
      product: myplatform
      provider: vaquero-plugin-vcoworkflow
      plugin_version: 0.1.0
      environment: qa1
      pools:
        default: &default
          reservation_policy: nonprod
          compute: small
          count: 2
      components:
        webserver:
          <<: *default

## Building

To build your platform, you do so an environment at a time:

    $ vaquero build qa1

You can also build a single component of your stack

    $ vaquero build qa1 --component webserver

Or even a single node

    $ vaquero build qa1 --component webserver --node 2

Or several specific nodes in a larger cluster

    $ vaquero build qa1 --component apiserver --node 2,5,8
