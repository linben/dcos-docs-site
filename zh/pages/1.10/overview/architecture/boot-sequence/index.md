---
layout: layout.pug
navigationTitle: Boot Sequence
title: Boot Sequence
menuWeight: 6
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

During installation, the DC/OS component services are all started in parallel but initialize and become responsive in a relatively consistent sequence because of interdependencies.

The DC/OS Diagnostics service monitors component service and node health. A node is marked as healthy when all its component services are healthy.

## Master nodes

The following is the boot sequence of DC/OS component services on each master node.

1. DC/OS Diagnostics starts 
    1. Polls systemd for component status
    2. Reports node unhealthy until all components (systemd services) are healthy
    3. Reports cluster unhealthy until all master nodes are healthy
2. Exhibitor starts 
    1. Creates ZooKeeper configuration and launches ZooKeeper
3. Mesos Master starts 
    1. Registers with local ZooKeeper
    2. Discovers other Mesos Masters from ZooKeeper
    3. Elects a leading master
4. Mesos-DNS starts 
    1. Discovers leading Mesos Master (from zk or local mesos-master?)
    2. Polls leading Mesos Master for cluster state
5. Networking components start 
    1. Forwards DNS lookups to Mesos-DNS
    2. Initializes VIP translation
    3. Initializes overlay network
6. Container orchestration components start 
    1. Registers with local ZooKeeper
    2. Elects a leading master
    3. Discovers leading Mesos Master from Mesos-DNS
    4. Leader registers with leading Mesos Master
7. Logging and Metrics components start
8. Security components start
9. Admin Router starts 
    1. Discovers leading Mesos Master from Mesos-DNS
    2. Discovers leading Marathon from Mesos-DNS
    3. Redirects unauthorized traffic to authentication components
    4. Proxies API and GUI traffic to discovered components
    5. Proxies admin service traffic to DC/OS services
    6. Serves DC/OS GUI

## Agent nodes

The following is the boot sequence of DC/OS components on each agent node.

1. DC/OS Diagnostics starts 
    1. Polls systemd for component status
    2. Reports node unhealthy until all components (systemd services) are healthy
2. Mesos Agent starts 
    1. Discovers leading Mesos Master from ZooKeeper
    2. Registers with leading Mesos Master
    3. Leading Mesos Master connects to the new agent using registered agent IP
    4. Leading Mesos Master starts offering the new agent's resources to schedulers for new tasks
    5. New DC/OS node become visible in the DC/OS API, GUI, and CLI
3. Networking components start 
    1. Forwards DNS lookups to Mesos-DNS
    2. Initializes VIP translation
    3. Initializes overlay network
4. Logging and Metrics components start
5. Admin Router Agent starts 
    1. Discovers leading Mesos Master from Mesos-DNS
    2. Proxies internal API traffic to discovered components

## Services

After DC/OS installation and initialization completes, you can install DC/OS services. Services can be installed through DC/OS package management from the Mesosphere Universe or through Marathon directly.

The following is the boot sequence of a DC/OS service.

1. Leading Mesos Master offers agent node resources to Marathon
2. Leading Marathon schedules the service onto agent nodes with sufficient resources
3. Mesos Agent starts the service as one or more containerized tasks

### Scheduler Services

Some DC/OS services are also schedulers that interact with DC/OS to manage tasks.

The following is the boot sequence of a DC/OS scheduler service.

1. Leading Mesos Master offers agent node resources to Marathon
2. Leading Marathon schedules the service onto agent nodes with sufficient resources
3. Mesos Agent starts the service as one or more containerized tasks
4. Scheduler service discovers leading Mesos Master from Mesos-DNS
5. Scheduler service registers with leading Mesos Master
6. Leading Mesos Master starts offering agent node resources to the new scheduler service

### More Information

For more information about tasks and services, see [Distributed Process Management](/1.10/overview/architecture/distributed-process-management/).