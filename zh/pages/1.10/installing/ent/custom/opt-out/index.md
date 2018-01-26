---
layout: layout.pug
navigationTitle: Opt-Out
title: Opt-Out
menuWeight: 501
excerpt: ""
enterprise: true
---
## Telemetry

You can opt-out of providing anonymous data by disabling [telemetry](/1.10/overview/telemetry/) for your cluster. To disable telemetry, add this parameter to your [`config.yaml`](/1.10/installing/ent/custom/configuration/configuration-parameters/) file during installation (note this requires using the [CLI](/1.10/installing/ent/custom/cli/) or [advanced](/1.10/installing/ent/custom/advanced/) installers):

`telemetry_enabled: 'false'`

If you’ve already installed your cluster and want to disable this in-place, you can go through an upgrade with the same parameter set.