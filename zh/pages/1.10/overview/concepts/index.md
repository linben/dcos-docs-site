---
layout: layout.pug
navigationTitle: 概念
title: 概念
menuWeight: 5
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# <a name="dcos-concepts"></a>DC/OS 概念

DC/OS由许多开源组件组成, 其中有几个是在 dc/os 之前存在的。 本文档中使用的术语可能与您熟悉的预先存在的术语类似, 但是, 它们可能与 DC/OS 使用不同的方式。

- [DC/OS](#dcos)
- [DC/OS GUI](#dcos-gui)
- [DC/OS CLI](#dcos-cli)
- [集群](#dcos-cluster)
- [网络](#network) 
  - [基础设施网络](#infrastructure-network)
  - [虚拟网络](#dcos-virtual-network)
- [节点](#dcos-node) 
  - [Master节点](#dcos-master-node)
  - [Agent节点](#dcos-agent-node) 
    - [Private Agent节点](#private-agent-node)
    - [Public Agent 节点](#public-agent-node)
- [主机操作系统](#host-operating-system)
- [自举机](#bootstrap-machine)
- [服务](#dcos-service) 
  - [马拉松服务](#marathon-service)
  - [Systemd 服务](#systemd-service)
  - [系统服务](#system-service)
  - [用户服务](#user-service)
- [服务组](#dcos-service-group)
- [工作](#dcos-job)
- [计划任务](#dcos-scheduler)
- [调度程序服务](#dcos-scheduler-service)
- [组件](#dcos-component)
- [包](#dcos-package)
- [软件包管理器](#dcos-package-manager)
- [包注册表](#dcos-package-registry)
- [Mesosphere Universe](#mesosphere-universe)
- [容器注册表](#container-registry)
- [云模板](#cloud-template)

### <a name="dcos"></a>DC/OS

DC/OS 是数据中心的 [ 分布式操作系统 ](https://en.wikipedia.org/wiki/Distributed_operating_system)。

- 与传统的分布式操作系统不同, DC/OS 也是一个容器平台, 它管理基于本机可执行文件或容器映像的集装箱化任务, 如 [ 泊坞窗图像 ](https://docs.docker.com/engine/tutorials/dockerimages/)。
- 与传统的[操作系统](https://en.wikipedia.org/wiki/Operating_system)不同，DC / OS运行在[节点集群](#cluster)，而不是一台机器。 每个DC / OS节点还有一个管理底层机器的[主机操作系统](#host-operating-system)。
- DC / OS由许多组件组成，最显着的是分布式系统内核([ Mesos ](#mesos)) 和容器编排引擎([ Marathon ](#marathon))。
- 在版本1.6 之前, DC/OS 被称为数据中心操作系统 (DCOS)。与版本1.6 平台被重命名了到 DC 或 OS 和开放来源。
- 虽然DC / OS本身是开源的，但像[ Mesosphere Enterprise DC / OS ](https://mesosphere.com/product/)这样的高级发行版可能包含其他封闭源组件和功能(例如多租户 ，细粒度的权限，秘密管理和端到端加密)。

### <a name="dcos-gui"></a>DC/OS GUI

[DC / OS图形用户界面(GUI)](/1.10/gui/)是用于从网络浏览器远程控制和管理DC / OS群集的接口。 GUI有时也被称为DC / OS UI或DC / OS Web界面。

### <a name="dcos-cli"></a>DC/OS CLI

[ DC / OS命令行界面(CLI)](/1.10/cli/)是一个用于从终端远程控制和管理DC / OS群集的界面。

### <a name="dcos-cluster"></a>集群

DC / OS群集是一组网络化的DC / OS节点，具有多个主节点和任意数量的公共和/或私人代理节点。

### <a name="network"></a>网络

DC / OS有两种类型的网络：基础设施网络和虚拟网络。

#### <a name="infrastructure-network"></a>基础设施网络

基础架构网络是由运行DC / OS的基础架构提供的物理或虚拟网络。 DC / OS不管理或控制此网络层，但要求DC / OS节点才能进行通信。

#### <a name="dcos-virtual-network"></a>虚拟网络

DC / OS虚拟网络是群集内部的虚拟网络，用于连接DC / OS上运行的DC / OS组件和容器化任务。

- 由DC / OS提供的虚拟网络是由虚拟网络服务(Navstar) 管理的VXLAN。
- 虚拟网络必须由管理员配置才能被任务使用。
- DC / OS上的任务可能会选择放置在特定的虚拟网络上，并给定容器特定的IP。
- 虚拟网络允许对在DC / OS上运行的任务进行逻辑细分。
- 虚拟网络上的每个任务都可以配置可选的地址组，将通信虚拟隔离到同一网络和地址组上的任务。

### <a name="dcos-node"></a>节点

DC / OS节点是运行Mesos代理和/或Mesos主进程的虚拟或物理机器。 DC / OS节点联网在一起形成一个DC / OS群集。

#### <a name="dcos-master-node"></a>Master节点

DC / OS master 点是运行一组DC / OS组件的虚拟或物理机器，它们一起工作来管理集群的其余部分。

- 每个主节点都包含多个DC / OS组件，其中最着名的是一个[ Mesos master ](#mesos-master)进程。
- 主节点在[法定人数](https://en.wikipedia.org/wiki/Quorum_%28distributed_computing%29)中工作，以提供集群协调的一致性。 为了避免[裂脑](https://en.wikipedia.org/wiki/Split-brain_%28computing%29)集群分区，集群应该总是有奇数个主节点。 例如，有三个主节点允许一个人下来; 有五个主节点允许两个关闭，允许在滚动更新期间失败。 额外的主节点可以添加额外的风险容忍度。
- 只有一个主节点的集群可用于开发，但不具备高可用性，可能无法从故障中恢复。

#### <a name="dcos-agent-node"></a>Agent节点

DC / OS agent 节点是运行Mesos任务的虚拟或物理机器。

- Each agent node contains multiple DC/OS components, including most notably a [Mesos agent](#mesos-agent) process.
- Agent nodes can be [private](#private-agent-node) or [public](#public-agent-node), depending on agent and network configuration.

For more information, see [Network Security](/1.10/administering-clusters/) and [Adding Agent Nodes](/1.10/administering-clusters/add-a-node/).

##### <a name="private-agent-node"></a>Private Agent节点

A private agent node is an agent node that is on a network that *does not* allow ingress from outside of the cluster via the cluster’s infrastructure networking.

- The Mesos agent on each private agent node is, by default, configured with none of its resources allocated to any specific Mesos roles (`*`).
- Most service packages install by default on private agent nodes.
- Clusters are generally comprised of mostly private agent nodes.

##### <a name="public-agent-node"></a>Public Agent 节点

A public agent node is an agent node that is on a network that *does* allow ingress from outside of the cluster via the cluster’s infrastructure networking.

- The Mesos agent on each public agent node is configured with the `public_ip:true` agent attribute and all of its resources allocated to the `slave_public` role.
- Public agent nodes are used primarily for externally facing reverse proxy load balancers, like [Marathon-LB](/service-docs/marathon-lb/).
- Clusters generally have only a few public agent nodes, because a single load balancer can handle proxying multiple services.

For more information, see [Converting Agent Node Types](/1.10/administering-clusters/convert-agent-type/).

### <a name="host-operating-system"></a>主机操作系统

A host operating system is the [operating system](https://en.wikipedia.org/wiki/Operating_system) that runs on each DC/OS node underneath the DC/OS components, manages the local hardware and software resources, and provides common services for running other programs and services.

- DC/OS currently supports the following host operating systems: [CentOS](https://www.centos.org/), [RHEL](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux), and [CoreOS](https://coreos.com/).
- While the host OS manges local tasks and machine resources, DC/OS manages cluster tasks and resources so that the user does not generally need to interact with the host operating systems on the nodes.

### <a name="bootstrap-machine"></a>自举机

A bootstrap machine is the machine on which the DC/OS installer artifacts are configured, built, and distributed.

- The bootstrap machine is not technically considered part of the cluster since it does not have DC/OS installed on it (this may change in the future). For most installation methods, the bootstrap node must be accessible to and from the machines in the cluster via infrastructure networking.
- The bootstrap machine is sometimes used as a jumpbox to control SSH access into other nodes in the cluster for added security and logging.
- One method of allowing master nodes to change IPs involves running ZooKeeper with Exhibitor on the bootstrap machine. Other alternatives include using S3, DNS, or static IPs, with various tradeoffs. For more information, see [configuring the exhibitor storage backend](/1.10/installing/custom/configuration/configuration-parameters/#exhibitor_storage_backend).
- If a bootstrap machine is not required for managing master node IP changes or as an SSH jumpbox, it can be shut down after bootstrapping and spun up on demand to [add new nodes](/1.10/administering-clusters/add-a-node/) to the cluster.

For more information, see the [system requirements](/1.10/installing/custom/system-requirements/#bootstrap-node).

### <a name="dcos-service"></a>服务

A DC/OS service is a set of one or more service instances that can be started and stopped as a group and restarted automatically if they exit before being stopped.

- Services is currently just a DC/OS GUI abstraction that translates to Marathon apps and pods in the CLI and API. This distinction will change over time as the name "service" is pushed upstream into component APIs.
- Sometimes "service" may also refer to a systemd service on the host OS. These are generally considered components and don’t actually run on Marathon or Mesos.
- A service may be either a system service or a user service. This distinction is new and still evolving as namespacing is transformed into a system-wide first class pattern.

#### <a name="marathon-service"></a>马拉松服务

A Marathon service consists of zero or more containerized service instances. Each service instance consists of one or more containerized Mesos tasks.

- Marathon apps and pods are both considered services. - Marathon app instances map 1 to 1 with tasks. - Marathon pod instances map 1 to many with tasks.
- Service instances are restarted as a new Mesos Task when they exit prematurely.
- Service instances may be re-scheduled onto another agent node if they exit prematurely and the agent is down or does not have enough resources anymore.
- Services can be installed directly via the [DC/OS API (Marathon)](/1.10/deploying-services/marathon-api/) or indirectly via the [DC/OS Package Manager (Cosmos)](#package-manager) from a [package repository](#dcos-package-repository) like [Mesosphere Universe](#mesosphere-universe). The [DC/OS GUI](#dcos-gui) and [DC/OS CLI](#dcos-cli) may be used to interact with the DC/OS Package Manager (Cosmos) more easily.
- A Marathon service may be a [DC/OS scheduler](#dcos-scheduler), but not all services are schedulers.
- A Marathon service is an abstraction around Marathon service instances which are an abstraction around Mesos tasks. Other schedulers (e.g. DC/OS Jobs (Metronome), Jenkins) have their own names for abstractions around Mesos tasks.

Examples: Cassandra (scheduler), Marathon-on-Marathon, Kafka (scheduler), Nginx, Tweeter.

#### <a name="systemd-service"></a>Systemd 服务

A systemd service is a service that consists of a single, optionally containerized, machine operating system process, running on the master or agent nodes, managed by systemd, owned by DC/OS itself.

- All systemd services are currently either host OS service, DC/OS dependencies, DC/OS components, or services manually managed by the system administrator.

Examples: Most DC/OS components, (system) Marathon.

#### <a name="system-service"></a>系统服务

A system service is a service that implements or enhances the functionality of DC/OS itself, run as either a Marathon service or a systemd service, owned by the system (admin) user or DC/OS itself.

- A system service may require special permissions to interact with other system services.
- Permission to operate as a system service on an Enterprise DC/OS cluster requires specific fine-grained permissions, while on open DC/OS all logged in users have the same administrative permissions.

Examples: All DC/OS components.

#### <a name="user-service"></a>用户服务

A user service is a Marathon service that is not a system service, owned by a user of the system.

- This distinction is new and still evolving as namespacing is transformed into a system-wide first class pattern and mapped to fine-grained user and user group permissions.

Examples: Jenkins, Cassandra, Kafka, Tweeter.

### <a name="dcos-service-group"></a>服务组

A DC/OS service group is a hierarchical (path-like) set of DC/OS services for namespacing and organization.

- Service groups are currently only available for Marathon services, not systemd services.
- This distinction may change as namespacing is transformed into a system-wide first class pattern.

### <a name="dcos-job"></a>工作

A DC/OS job is a set of similar short-lived job instances, running as Mesos tasks, managed by the DC/OS Jobs (Metronome) component.

- A job can be created to run only once, or may run regularly on a schedule.

### <a name="dcos-scheduler"></a>计划任务

A DC/OS scheduler is a Mesos scheduler that runs as a systemd service on master nodes or Mesos task on agent nodes.

- The key differences between a DC/OS scheduler and Mesos scheduler are where it runs and how it is installed.
- Some schedulers come pre-installed as DC/OS components (e.g. Marathon, DC/OS Jobs (Metronome)).
- Some schedulers can be installed by users as user services (e.g Kafka, Cassandra).
- Some schedulers run as multiple service instances to provide high availability (e.g. Marathon).
- In certain security modes within Enterprise DC/OS, a DC/OS scheduler must authenticate and be authorized using a service account to register with Mesos as a framework.

### <a name="dcos-scheduler-service"></a>调度程序服务

A DC/OS scheduler service is a long-running DC/OS scheduler that runs as a DC/OS service (Marathon or systemd).

- Since DC/OS schedulers can also be run as short-lived tasks, not all schedulers are services.

### <a name="dcos-component"></a>组件

A DC/OS component is a DC/OS system service that is distributed with DC/OS.

- Components may be systemd services or Marathon services.
- Components may be deployed in a high availability configuration.
- Most components run on the master nodes, but some (e.g. mesos-agent) run on the agent nodes.

Examples: Mesos, Marathon, Mesos-DNS, Bouncer, Admin Router, DC/OS Package Manager (Cosmos), History Service, etc.

### <a name="dcos-package"></a>包

A DC/OS package is a bundle of metadata that describes how to configure, install, and uninstall a DC/OS service using Marathon.

### <a name="dcos-package-manager"></a>软件包管理器

The [DC/OS Package Manager (Cosmos)(https://github.com/dcos/cosmos)) is a component that manages installing and uninstalling packages on a DC/OS cluster.

- The DC/OS GUI and DC/OS CLI act as clients to interact with the DC/OS Package Manager.
- The [DC/OS Package Manager API](https://github.com/dcos/cosmos) allows programmatic interaction.

### <a name="dcos-package-registry"></a>包注册表

A DC/OS package registry is a repository of DC/OS packages.

- The [DC/OS Package Manager](#dcos-package-manager) may be configured to install packages from one or more package registries.

### <a name="mesosphere-universe"></a>Mesosphere Universe

The Mesosphere Universe is a public package registry, managed by Mesosphere.

For more information, see the [Universe repository](https://github.com/mesosphere/universe) on GitHub.

### <a name="container-registry"></a>容器注册表

A container registry is a repository of pre-built container images.

The [Universal Container Runtime](#mesos-containerizer-universal-container-runtime) and [Docker Engine](#mesos-containerizer-docker-runtime) can both run Docker images from public or private Docker container registries.

### <a name="cloud-template"></a>云模板

A cloud template is an infrastructure-specific method of declaratively describing a DC/OS cluster.

For more information, see [Cloud Installation Options](/1.10/installing/cloud/).

# <a name="mesos-concepts"></a>Mesos概念

The following terms are contextually correct when talking about Apache Mesos, but may be hidden by other abstraction within DC/OS.

- [Apache Mesos](#apache-mesos)
- [Master](#mesos-master)
- [Agent](#mesos-agent)
- [任务](#mesos-task)
- [Executor](#mesos-executor)
- [计划任务](#mesos-scheduler)
- [框架](#mesos-framework)
- [角色](#mesos-role)
- [Resource Offer](#mesos-resource-offer)
- [Containerizer](#mesos-containerizer) 
  - [Universal Container Runtime](#mesos-containerizer-universal-container-runtime)
  - [Docker Engine](#mesos-containerizer-docker-engine)
- [Exhibitor & ZooKeeper](#mesos-exhibitor-zookeeper)
- [Mesos\-DNS](#mesos-dns)

### <a name="apache-mesos"></a>Apache Mesos

Apache Mesos is a distributed systems kernel that manages cluster resources and tasks.

- Mesos is one of the core components of DC/OS that predates DC/OS itself, bringing maturity and stability to the platform.

For more information, see the [Mesos website](http://mesos.apache.org/).

### <a name="mesos-master"></a>Master

A Mesos master is a process that runs on master nodes to coordinate cluster resource management and facilitate orchestration of tasks.

- The Mesos masters form a quorum and elect a leader.
- The lead Mesos master collects resources reported by Mesos agents and makes resource offers to Mesos schedulers. Schedulers then may accept resource offers and place tasks on their corresponding nodes.

### <a name="mesos-agent"></a>Agent

A Mesos agent is a process that runs on agent nodes to manage the executors, tasks, and resources of that node.

- The Mesos agent registers some or all of the node’s resources, which allows the lead Mesos master to offer those resources to schedulers, which decide on which node to run tasks.
- The Mesos agent reports task status updates to the lead Mesos master, which in turn reports them to the appropriate scheduler.

### <a name="mesos-task"></a>任务

A Mesos task is an abstract unit of work, lifecycle managed by a Mesos executor, that runs on a DC/OS agent node.

- Tasks are often processes or threads, but could even just be inline code or items in a single-threaded queue, depending on how their executor is designed.
- The Mesos built-in command executor runs each task as a process that can be containerized by one of several [Mesos containerizers](#mesos-containerizer).

### <a name="mesos-executor"></a>Executor

A Mesos executor is a method by which Mesos agents launch tasks.

- Executor processes are launched and managed by Mesos agents on the agent nodes.
- Mesos tasks are defined by their scheduler to be run by a specific executor (or the default executor).
- Each executor runs in its own container.

For more information about framework schedulers and executors, see the [Application Framework development guide](http://mesos.apache.org/documentation/latest/app-framework-development-guide/).

### <a name="mesos-scheduler"></a>Scheduler

A Mesos scheduler is a program that defines new Mesos tasks and assigns resources to them (placing them on specific nodes).

- A scheduler receives resource offers describing CPU, RAM, etc., and allocates them for discrete tasks that can be launched by Mesos agents.
- A scheduler must register with Mesos as a framework.

Examples: Kafka, Marathon, Cassandra.

### <a name="mesos-framework"></a>Framework

A Mesos framework consists of a scheduler, tasks, and optionally custom executors.

- The term framework and scheduler are sometimes used interchangeably. Prefer scheduler within the context of DC/OS.

For more information about framework schedulers and executors, see the [Application Framework development guide](http://mesos.apache.org/documentation/latest/app-framework-development-guide/).

### <a name="mesos-role"></a>Role

A Mesos role is a group of Mesos frameworks that share reserved resources, persistent volumes, and quota. These frameworks are also grouped together in Mesos' hierarchical Dominant Resource Fairness (DRF) share calculations.

- Roles are often confused as groups of resources, because of the way they can be statically configured on the agents. The assignment is actually the inverse: resources are assigned to roles.
- Role resource allocation can be configured statically on the Mesos agent or changed at runtime using the Mesos API.

### <a name="mesos-resource-offer"></a>Resource Offer

A Mesos resource offer provides a set of unallocated resources (e.g. cpu, disk, memory) from an agent to a scheduler so that the scheduler may allocate those resources to one or more tasks. Resource offers are constructed by the leading Mesos master, but the resources themselves are reported by the individual agents.

### <a name="mesos-containerizer"></a>Containerizer

A containerizer provides a containerization and resource isolation abstraction around a specific container runtime. The supported runtimes are the Universal Container Runtime and Docker Engine.

#### <a name="mesos-containerizer-universal-container-runtime"></a>Universal Container Runtime

The Universal Container Runtime launches Mesos containers from binary executables and Docker images. Mesos containers managed by the Universal Container Runtime do not use Docker Engine, even if launched from a Docker image.

#### <a name="mesos-containerizer-docker-engine"></a>Docker Engine

The [Docker Engine](https://www.docker.com/products/docker-engine) launches Docker containers from Docker images.

### <a name="mesos-exhibitor-zookeeper"></a>Exhibitor & ZooKeeper

Mesos depends on ZooKeeper, a high-performance coordination service to manage the cluster state. Exhibitor automatically configures and manages ZooKeeper on the [master nodes](#master-node).

### <a name="mesos-exhibitor-zookeeper"></a>Mesos-DNS

Mesos-DNS is a DC/OS component that provides service discovery within the cluster. Mesos-DNS allows applications and services that are running on Mesos to find each other by using the domain name system (DNS), similar to how services discover each other throughout the Internet.

For more information, see the [Mesos-DNS documentation](/1.10/networking/mesos-dns/).

# <a name="marathon-concepts"></a>马拉松概念

The following terms are contextually correct when talking about Marathon, but may be hidden by other abstraction within DC/OS.

- [Marathon](#marathon)
- [Application](#marathon-application)
- [Pod](#marathon-pod)
- [Group](#marathon-group)

### <a name="marathon"></a>马拉松

Marathon is a container orchestration engine for Mesos and DC/OS.

- Marathon is one of the core components of DC/OS that predates DC/OS itself, bringing maturity and stability to the platform.

For more information, see the [Marathon website](https://mesosphere.github.io/marathon/).

### <a name="marathon-application"></a>报名

A Marathon application is a long-running service that may have one or more instances that map one to one with Mesos tasks.

- The user creates an application by providing Marathon with an application definition (JSON). Marathon then schedules one or more application instances as Mesos tasks, depending on how many the definition specified.
- Applications currently support the use of either the [Mesos Universal Container Runtime](#mesos-universal-container-runtime) or the [Docker Runtime](#mesos-docker-runtime).

### <a name="marathon-pod"></a>Pod

A Marathon pod is a long-running service that may have one or more instances that map one to many with colocated Mesos tasks.

- The user creates a pod by providing Marathon with a pod definition (JSON). Marathon then schedules one or more pod instances as Mesos tasks, depending on how many the definition specified.
- Pod instances may include one or more tasks that share certain resources (e.g. IPs, ports, volumes).
- Pods currently require the use of the [Mesos Universal Container Runtime](#mesos-universal-container-runtime).

### <a name="marathon-group"></a>群组

A Marathon group is a set of services (applications and/or pods) within a hierarchical directory [path](https://en.wikipedia.org/wiki/Path_%28computing%29) structure for namespacing and organization.