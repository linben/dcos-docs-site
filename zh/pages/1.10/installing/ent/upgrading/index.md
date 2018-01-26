---
layout: layout.pug
navigationTitle: 升级
title: 升级
menuWeight: 4
excerpt: ""
enterprise: true
---
This document provides instructions for upgrading a DC/OS cluster.

如果此升级是在满足所有先决条件的支持的操作系统上执行的，则此升级*应*保留群集上正在运行的任务的状态。 本文档重用了部分[高级DC / OS安装指南](/1.10/installing/ent/custom/advanced/)。

**重要：**

- Review the [release notes](/1.10/release-notes/) before upgrading DC/OS.
- 在升级所有主节点之前, DC/OS GUI 和其他更高级别的系统 api 可能不一致或不可用。 例如, 升级后的 DC/OS 马拉松领先者无法连接到领先的 Mesos 主机, 直到它也被升级。 发生此情况时:
    
    - DC/OS GUI 可能无法提供准确的服务列表。
    - 对于多主配置, 在一个主服务器完成升级后, 您可以从8181端口上的展商用户界面监视其余主机的运行状况。
- 升级后的 DC/OS 马拉松领先者无法连接到非安全 (即不升级) 领先的 Mesos 主机。 在升级所有主控形状之前, 不能信任 DC/OS UI。 有多个马拉松计划程序实例和多个 Mesos 的主控形状, 每个都在升级, 而马拉松的领导者可能不是 Mesos 的领导者。
- Mesos UI 中的任务历史记录不会通过升级而持续。
- 在您从1.9 升级到1.10 之前, 您必须升级马拉松-LB。通过卸载马拉松-LB 并重新安装最新的软件包来做到这一点。
- 可以在 [ 此处 ](https://support.mesosphere.com/hc/en-us/articles/213198586-Mesosphere-Enterprise-DC-OS-Downloads) 找到 DC/OS 企业下载。

## 支持的升级路径

- 从最新的 ga 版本到最新的 ga 版本的电流。例如, 如果1.8.8 是最新的, 并且1.9.0 是最新的, 则支持此升级。 
    - [1.7 to 1.8](/1.8/administration/upgrading/)
    - [1.8 to 1.9](/1.9/installing/ent/upgrading/)
    - [1.9 to 1.10](/1.10/installing/ent/upgrading/)
- 从任何当前版本到下一个。例如, 将支持从1.9.1 到1.9.2 的升级。
- 从任何当前版本到相同的版本。例如, 将支持从1.9.0 到1.9.0 的升级。这对于进行配置更改很有用。

## 修改DC / OS配置

You *cannot* change your cluster configuration at the same time as upgrading to a new version. 对群集配置的更改必须通过对已安装版本的更新来完成。 例如，不能同时将群集从1.10.x升级到1.10.y，并添加更多的公共代理。 您可以添加更多的公共代理程序，更新到1.10.x，然后升级到1.10.y. 或者您可以升级到1.10.y，然后在升级后通过更新1.10.y添加更多的公共代理。 上下文| 编辑上下文

要修改DC / OS配置，必须使用修改的` config.yaml `运行安装程序，并使用新的安装文件更新群集。 对DC / OS配置的更改与升级主机的风险相同。 不正确的配置可能会导致主机或整个群集崩溃。

只能修改DC / OS配置参数的一个子集。 对DC / OS上运行的任何软件的不利影响超出了本文的范围。 联系Mesosphere支持了解更多信息。

以下是您可以修改的参数列表：

- [`dns_search`](/1.10/installing/ent/custom/configuration/configuration-parameters/#dns-search)
- [`docker_remove_delay`](/1.10/installing/ent/custom/configuration/configuration-parameters/#docker-remove-delay)
- [`gc_delay`](/1.10/installing/ent/custom/configuration/configuration-parameters/#gc-delay)
- [`resolvers`](/1.10/installing/ent/custom/configuration/configuration-parameters/#resolvers)
- [`telemetry_enabled`](/1.10/installing/ent/custom/configuration/configuration-parameters/#telemetry-enabled)
- [`use_proxy`](/1.10/installing/ent/custom/configuration/configuration-parameters/#use-proxy) 
    - [`http_proxy`](/1.10/installing/ent/custom/configuration/configuration-parameters/#use-proxy)
    - [`https_proxy`](/1.10/installing/ent/custom/configuration/configuration-parameters/#use-proxy)
    - [`no_proxy`](/1.10/installing/ent/custom/configuration/configuration-parameters/#use-proxy)

安全模式(`security`) 可以更改，但有特殊的注意事项。

- 您只能更新到更严格的安全模式。 Security downgrades are not supported. For example, if your cluster is in `permissive` mode and you want to downgrade to `disabled` mode, you must reinstall the cluster and terminate all running workloads.
- During each update, you can only increase your security by a single level. For example, you cannot update directly from `disabled` to `strict` mode. To increase from `disabled` to `strict` mode you must first update to `permissive` mode, and then update from `permissive` to `strict` mode.

See the security [mode](/1.10/installing/ent/custom/configuration/configuration-parameters/#security-enterprise) for a description of the different security modes and what each means.

# 指导

These steps must be performed for version upgrades and cluster configuration changes.

## Prerequisites

- Mesos, Mesos Frameworks, Marathon, Docker and all running tasks in the cluster should be stable and in a known healthy state.
- For Mesos compatibility reasons, we recommend upgrading any running Marathon-on-Marathon instances to Marathon version 1.3.5 before proceeding with this DC/OS upgrade.
- You must have access to copies of the config files used with the previous DC/OS version: `config.yaml` and `ip-detect`.
- You must be using systemd 218 or newer to maintain task state.
- All hosts (masters and agents) must be able to communicate with all other hosts on all ports, for both TCP and UDP.
- In CentOS or RedHat, install IP sets with this command (used in some IP detect scripts): `sudo yum install -y ipset`
- You must be familiar with using `systemctl` and `journalctl` command line tools to review and monitor service status. Troubleshooting notes can be found at the end of this [document](#troubleshooting).
- You must be familiar with the [Advanced DC/OS Installation Guide](/1.10/installing/ent/custom/advanced/).
- Take a snapshot of ZooKeeper prior to upgrading. Marathon supports rollbacks, but does not support downgrades.
- Ensure that Marathon event subscribers are disabled before beginning the upgrade. Leave them disabled after completing the upgrade, as this feature is now deprecated.
- Verify that all Marathon application constraints are valid before beginning the upgrade. Use [this script](https://github.com/mesosphere/public-support-tools/blob/master/check-constraints.py) to check if your constraints are valid.
- [Back up your cluster](/1.10/administering-clusters/backup-and-restore/).
- Optional: You can add custom [node and cluster healthchecks](/1.10/installing/ent/custom/node-cluster-health-check/#custom-health-checks) to your `config.yaml`.

## Bootstrap Node

Choose your desired security mode and then follow the applicable upgrade instructions.

- [Installing DC/OS 1.10 without changing security mode](#current-security)
- [Installing DC/OS 1.10 in permissive mode](#permissive)
- [Installing DC/OS 1.10 in strict mode](#strict)

# <a name="current-security"></a>Installing DC/OS 1.10 without changing security mode

This procedure upgrades a DC/OS 1.9 cluster to DC/OS 1.10 without changing the cluster's [security mode](/1.10/installing/ent/custom/configuration/configuration-parameters/#security-enterprise).

1. Copy your existing `config.yaml` and `ip-detect` files to an empty `genconf` folder on your bootstrap node. The folder should be in the same directory as the installer.
2. Merge the old `config.yaml` into the new `config.yaml` format. In most cases the differences will be minimal.
    
    **Important:**
    
    - You cannot change the `exhibitor_zk_backend` setting during an upgrade.
    - The syntax of the `config.yaml` may be different from the earlier version. For a detailed description of the current `config.yaml` syntax and parameters, see the [documentation](/1.10/installing/ent/custom/configuration/configuration-parameters/).
3. After updating the format of the config.yaml, compare the old config.yaml and new config.yaml. Verify that there are no differences in pathways or configurations. Changing these while upgrading can lead to catastrophic cluster failures.
4. Modify the `ip-detect` file as desired.
5. Build your installer package.
    
    1. Download the `dcos_generate_config.ee.sh` file.
    2. Generate the installation files. Replace `<installed_cluster_version>` in the below command with the DC/OS version currently running on the cluster you intend to upgrade, for example `1.8.8`. 
            bash
            dcos_generate_config.ee.sh --generate-node-upgrade-script <installed_cluster_version>
    
    3. The command in the previous step will produce a URL in the last line of its output, prefixed with `Node upgrade script URL:`. Record this URL for use in later steps. It will be referred to in this document as the "Node upgrade script URL".
    4. Run the [nginx](/1.10/installing/ent/custom/advanced/) container to serve the installation files.

6. Go to the DC/OS Master [procedure](#masters) to complete your installation.

# <a name="permissive"></a>Installing DC/OS 1.10 in permissive mode

This procedure upgrades to DC/OS 1.10 in [permissive security mode](/1.10/installing/ent/custom/configuration/configuration-parameters/#security-enterprise).

**Prerequisite:**

- Your cluster must be [upgraded to DC/OS 1.10](#current-security) and running in [disabled security mode](/1.10/installing/ent/custom/configuration/configuration-parameters/#security-enterprise) before it can be upgraded to permissive mode. If your cluster was running in permissive mode before it was upgraded to DC/OS 1.10, you can skip this procedure.

**Important:** Any [custom node or cluster healthchecks](/1.10/installing/ent/custom/node-cluster-health-check/#custom-health-checks) you have configured will fail for an upgrade from disabled to permissive security mode. A future release will allow you to bypass the healthchecks.

To update a cluster from disabled security to permissive security, complete the following procedure:

1. Replace `security: disabled` with `security: permissive` in your `config.yaml`. Do not make any other changes to pathways or configurations in the `config.yaml`.
2. Modify the `ip-detect` file as desired.
3. Build your installer package.
    
    1. Download the `dcos_generate_config.ee.sh` file.
    2. Generate the installation files. Replace `<installed_cluster_version>` in the below command with the DC/OS version currently running on the cluster you intend to upgrade, for example `1.8.8`. 
            bash
            dcos_generate_config.ee.sh --generate-node-upgrade-script <installed_cluster_version>
    
    3. The command in the previous step will produce a URL in the last line of its output, prefixed with `Node upgrade script URL:`. Record this URL for use in later steps. It will be referred to in this document as the "Node upgrade script URL".
    4. Run the [nginx](/1.10/installing/ent/custom/advanced/) container to serve the installation files.

4. Go to the DC/OS Master [procedure](#masters) to complete your installation.

# <a name="strict"></a>Installing DC/OS 1.10 in strict mode

This procedure upgrades to DC/OS 1.10 in security strict [mode](/1.10/installing/ent/custom/configuration/configuration-parameters/#security-enterprise).

If you are updating a running DC/OS cluster to run in `security: strict` mode, beware that security vulnerabilities may persist even after migration to strict mode. When moving to strict mode, your services will now require authentication and authorization to register with Mesos or access its HTTP API. You should test these configurations in permissive mode before upgrading to strict, to maintain scheduler and script uptimes across the upgrade.

**Prerequisites:**

- Your cluster must be [upgraded to DC/OS 1.10](#current-security) and running in [permissive security mode](#permissive) before it can be updated to strict mode. If your cluster was running in strict mode before it was upgraded to DC/OS 1.10, you can skip this procedure.
- If you have running pods or if the Mesos "HTTP command executors" feature has been enabled in a custom configuration, you must restart these tasks in DC/OS 1.10 permissive security mode before upgrading to strict mode. Otherwise, these tasks will be restarted when the masters are upgraded.

To update a cluster from permissive security to strict security, complete the following procedure:

1. Replace `security: permissive` with `security: strict` in your `config.yaml`. Do not make any other changes to pathways or configurations in the `config.yaml`.
2. Modify the `ip-detect` file as desired.
3. Build your installer package.
    
    1. Download the `dcos_generate_config.ee.sh` file.
    2. Generate the installation files. Replace `<installed_cluster_version>` in the below command with the DC/OS version currently running on the cluster you intend to upgrade, for example `1.8.8`. 
            bash
            dcos_generate_config.ee.sh --generate-node-upgrade-script <installed_cluster_version>
    
    3. The command in the previous step will produce a URL in the last line of its output, prefixed with `Node upgrade script URL:`. Record this URL for use in later steps. It will be referred to in this document as the "Node upgrade script URL".
    4. Run the [nginx](/1.10/installing/ent/custom/advanced/) container to serve the installation files.

4. Go to the DC/OS Master [procedure](#masters) to complete your installation.

## <a name="masters"></a>DC/OS Masters

Proceed with upgrading every master node one-at-a-time in any order using the following procedure. When you complete each upgrade, monitor the Mesos master metrics to ensure the node has rejoined the cluster and completed reconciliation.

1. Download and run the node upgrade script:
    
    ```bash
curl -O <Node upgrade script URL>
sudo bash dcos_node_upgrade.sh
```

2. Verify that the upgrade script succeeded and exited with the status code ``:
    
    ```bash
echo $?
0
```

3. Validate the upgrade:
    
    1. Monitor Exhibitor and wait for it to converge at `http://<master-ip>:8181/exhibitor/v1/ui/index.html`. Confirm that the master rejoins the ZooKeeper quorum successfully (the status indicator will turn green).
        
        **Tip:** If you are upgrading from permissive to strict mode, this URL will be `https://...`.
    
    2. Wait until the `dcos-mesos-master` unit is up and running.
    3. Verify that `curl http://<dcos_master_private_ip>:5050/metrics/snapshot` has the metric `registrar/log/recovered` with a value of `1`. **Tip:** If you are upgrading from permissive to strict mode, this URL will be `curl https://...` and you will need a JWT for access.
    4. Verify that `/opt/mesosphere/bin/mesos-master --version` indicates that the upgraded master is running Mesos 1.2.0.

4. Go to the DC/OS Agents [procedure](#agents) to complete your installation.

## <a name="agents"></a>DC/OS Agents

**Important:** When upgrading agent nodes, there is a 5 minute timeout for the agent to respond to health check pings from the mesos-masters before it is considered lost and its tasks are given up for dead.

On all DC/OS agents:

1. Navigate to the `/opt/mesosphere/lib` directory and delete this library file. Deleting this file will prevent conflicts.
    
    ```bash
  libltdl.so.7
```

2. Download and run the node upgrade script:
    
    ```bash
curl -O <Node upgrade script URL>
sudo bash dcos_node_upgrade.sh
```

3. Verify that the upgrade script succeeded and exited with the status code ``:
    
    ```bash
echo $?
0
```

4. Validate the upgrade:
    
    - Verify that `curl http://<dcos_agent_private_ip>:5051/metrics/snapshot` has the metric `slave/registered` with a value of `1`.
    - Monitor the Mesos UI to verify that the upgraded node rejoins the DC/OS cluster and that tasks are reconciled (`http://<master-ip>/mesos`). If you are upgrading from permissive to strict mode, this URL will be `https://<master-ip>/mesos`.

## <a name="troubleshooting"></a>Troubleshooting Recommendations

The following commands should provide insight into upgrade issues:

### On All Cluster Nodes

```bash
sudo journalctl -u dcos-download
sudo journalctl -u dcos-spartan
sudo systemctl | grep dcos
```

If your upgrade fails because of a [custom node or cluster check](/1.10/installing/ent/custom/node-cluster-health-check/#custom-health-checks), run these commands for more details:

```bash
dcos-diagnostics check node-poststart
dcos-diagnostics check cluster
```

### On DC/OS Masters

```bash
sudo journalctl -u dcos-exhibitor
less /opt/mesosphere/active/exhibitor/usr/zookeeper/zookeeper.out
sudo journalctl -u dcos-mesos-dns
sudo journalctl -u dcos-mesos-master
```

### On DC/OS Agents

```bash
sudo journalctl -u dcos-mesos-slave
```

## Notes:

- Packages available in the DC/OS 1.10 Universe are newer than those in the older versions of Universe. Services are not automatically upgraded when DC/OS is installed because not all DC/OS services have upgrade paths that will preserve existing state.