---
layout: layout.pug
navigationTitle:  Limitations
title: Limitations
menuWeight: 100
excerpt:
featureMaturity:
enterprise: false
---

<!-- This source repo for this topic is https://github.com/mesosphere/dcos-commons -->


## Node Count

The DC/OS Cassandra Service must be deployed with at least 3 nodes.

## Rack-Aware Placement

Apache Cassandra's Rack-Aware Replication is not supported at this time.

## Security

Apache Cassandra's native TLS, authentication, and authorization features are not supported at this time.

## Data Center Name

The name of the data center cannot be changed after installation. `service.data_center` and `service.rack` options are not allowed to be modified once Cassandra is installed.

```
"service": {
...
        data_center": {
          "description": "The name of the data center this cluster is running in",
          "type": "string",
          "default": "datacenter1"
        },
        "rack": {
          "description": "The name of the rack this cluster is running on",
          "type": "string",
          "default": "rack1"
        },
...
}
```

## Service user

The default user is `nobody`. However, if the UID of `nobody` is not `65534` on the host agents, then Cassandra must be run as `root`.

To determine the UID of `nobody`, run the command `id nobody`. The output of which is:
```
uid=65534(nobody) gid=65534(nobody) groups=65534(nobody)
```

If the returned UID is not `65534`, then Cassandra can be installed as root by setting the service user at install time:
```
"service": {
        "user": "root",
        ...
}
...
```

## Out-of-band configuration

Out-of-band configuration modifications are not supported. The service's core responsibility is to deploy and maintain the service with a specified configuration. In order to do this, the service assumes that it has ownership of task configuration. If an end-user makes modifications to individual tasks through out-of-band configuration operations, the service will override those modifications at a later time. For example:
- If a task crashes, it will be restarted with the configuration known to the scheduler, not one modified out-of-band.
- If a configuration update is initiated, all out-of-band modifications will be overwritten during the rolling update.

## Scaling in

To prevent accidental data loss, the service does not support reducing the number of pods.

## Disk changes

To prevent accidental data loss from reallocation, the service does not support changing volume requirements after initial deployment.

## Best-effort installation

If your cluster doesn't have enough resources to deploy the service as requested, the initial deployment will not complete until either those resources are available or until you reinstall the service with corrected resource requirements. Similarly, scale-outs following initial deployment will not complete if the cluster doesn't have the needed available resources to complete the scale-out.

## Virtual networks

When the service is deployed on a virtual network, the service may not be switched to host networking without a full re-installation. The same is true for attempting to switch from host to virtual networking.
