---
layout: page
title: Plugins
permalink: /docs/plugins.html
---

Vaquero has a plugin architecture in order to allow it to target multiple
types of infastructure services.

To install a plugin:

```
$ vaquero plugin install <URL>
```

## Plugins

- [`vaquero-plugin-test`](plugins/test.html) - A test plugin for verifying `vaquero` code behavior
- [`vaquero-plugin-vcoworkflow`](plugins/vcoworkflow.html) - A plugin for provisioning via a VMware vRealize Orchestrator workflow. This requires a number of workflows to be present in your Orchestrator library. More information to come soon.
