DC/OS可以运行许多不同类型的由任务组成的工作负载。

DC/OS任务是由DC/OS内置调度程序或在DC/OS上运行的调度程序服务调度的[Mesos任务](/1.10/overview/concepts/#mesos-task)。

# 执行程序

任务在启动任务时由[scheduler](/1.10/overview/concepts/#dcos-scheduler)指定的[Mesos Executor](/1.10/overview/concepts/#mesos-executor)执行。 在Mesos中，调度程序和执行程序被称为[framework](/1.10/overview/concepts/#mesos-framework)，但是在广义的DC/OS的概念下，经常被称为“调试器” ，“执行程序”和“任务”。

### 内置执行程序

Mesos包括所有调度程序都可以使用的内置执行程序，但调度程序也可以使用自己的执行程序。

- 命令执行程序 - 执行shell命令或Docker容器
- 默认执行程序(Mesos 1.1) - 执行一组shell命令或Docker容器

有关Mesos执行程序的更多信息，请参见[Mesos框架开发指南](https://mesos.apache.org/documentation/latest/app-framework-development-guide/)

## 调度程序

由于任务系统是通用的，用户一般不直接创建任务或与任务交互。相反，调度程序通常提供更高层次的抽象。

### 内置调度程序

DC / OS有两个内置调度程序：

- Marathon调度程序提供**服务**(Apps和Pods)，并行持续运行。
- Metronome调度程序提供**任务**，可立即运行或按设定的时间表运行。

有关Marathon服务的更多信息，请参阅[Services docs](/1.10/deploying-services/)或[Marathon docs](https://mesosphere.github.io/marathon/docs/)。

有关Metronome任务的更多信息，请参阅[Jobs docs](/1.10/deployment-jobs/)。

### 用户空间调度程序

其他的调度程序可以作为[scheduler services](/1.10/overview/concepts/#dcos-scheduler-service)安装 在Marathon上，或从[Mesosphere Universe](/1.10/overview/concepts/＃mesosphere-universe)或直接通过Marathon安装。

用户空间调度程序示例：

- Kafka调度程序提供**Kafka brokers**，作为管理Kafka节点生命周期的节点运行。
- Cassandra调度程序提供**Cassandra节点**，作为管理Cassandra节点生命周期的节点运行。
- Spark调度程序(分配程序)提供** Spark作业**，它们本身是Spark任务的调度程序。

有关可安装调度程序(和其他程序包)的完整列表，请参阅[Mesosphere Universe程序包列表](https://universe.dcos.io/#/)。
