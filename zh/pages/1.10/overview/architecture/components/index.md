DC/OS是由许多开源微服务组件精心调整和配置协同工作。

Mesosphere DC/OS企业版包含大多数开源DC/OS组件，同时也包括一些附加组件，模块和插件。

![Mesosphere DC/OS Enterprise Components](/1.10/img/dcos-enterprise-components-1.10-portrait.png)

从上面看，DC/OS是一个包含电池的容器平台，可以完成容器编排，包管理和安全。

从底层看，DC/OS是一个基于[Apache Mesos]（http://mesos.apache.org/）构建的操作系统，用于处理集群管理和软件定义网络，同时简化日志记录和性能度量收集。

#集群管理

DC/OS作为一个集群级系统，提供了一种查看和操作大量单机级系统的方法。它隐藏了分布式系统内核Mesos的复杂性，具有更高层次的抽象，接口和工具。 集群管理是功能核心，同时包括内核，依赖关系和用户界面。

<div data-role="collapsible">
<h2 id =“apache-mesos”> Apache Mesos </h2>
<DIV>
<p><strong>描述:</strong> Mesos以分布式系统内核的方式管理资源和任务。 Mesos主节点对外暴露调度程序，执行程序和操作界面以完成集群管理。 Mesos代理节点各自管理运行在每个DC/OS代理节点<a href="/1.10/overview/concepts/#dcos-agent-node"> </a>上的执行程序，任务和资源。 Mesos Agent Public是配置在<a href="/1.10/overview/concepts/#public-agent-node"> DC/OS公共代理节点</a>上运行的Mesos代理程序。</p>
<P>
  <strong>系统服务：</strong>
  <UL>
    <li> <code class =“nowrap”> dcos-mesos-master.service </code> </li>
    <li> <code class =“nowrap”> dcos-mesos-slave.service </code> </li>
    <li> <code class =“nowrap”> dcos-mesos-slave-public.service </code> </li>
  </UL>
</p>
<P>
  <strong>另请参阅：</strong>
  <UL>
    <li> <a href="http://mesos.apache.org/">文档</a> </li>
    <li> <a href="https://github.com/apache/mesos">源代码</a> </li>
    <li> <a href="https://mesos.apache.org/documentation/latest/endpoints/"> API参考资料</a> </li>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="apache-zookeeper">Apache ZooKeeper</h2>
<div>
<p><strong>描述:</strong>ZooKeeper为配置，同步，名称注册和集群状态保存提供了一致的，高可用的，分布式键值存储。</p>
<p><strong>系统服务:</strong> N/A - ZooKeeper由Exhibitor服务管理.</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://zookeeper.apache.org/">文档</a></li>
    <li><a href="https://github.com/apache/zookeeper">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="exhibitor">Exhibitor</h2>
<div>
<p><strong>描述:</strong> Exhibitor管理ZooKeeper并提供一个管理的web界面.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-exhibitor.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/soabase/exhibitor/wiki">文档</a></li>
    <li><a href="https://github.com/dcos/exhibitor">源代码</a></li>
    <li><a href="https://github.com/soabase/exhibitor/wiki/REST-Introduction">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-installer">DC/OS Installer</h2>
<div>
<p><strong>描述:</strong>DC/OS安装程序（dcos_generate_config.ee.sh）生成安装程序并安装DC/OS。作为每个节点上安装过程的一部分，DC/OS下载服务从引导计算机上下载安装组件，DC/OS配置服务使用DC/OS组件包管理器（Pkgpanda）和安装组件。</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-download.service</code></li>
    <li><code class="nowrap">dcos-setup.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="/1.10/installing/oss/">文档</a></li>
    <li><a href="https://github.com/dcos/dcos">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-backup">DC/OS Backup <span class="badge badge--content-header badge--enterprise">ENTERPRISE</span></h2>
<div>
<p><strong><em>1.10.0新加功能</em></strong></p>
<p><strong>描述:</strong> DC/OS Backup提供备份和恢复DC/OS组件状态(在1.10中仅限Marathon).</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-backup-master.service</code></li>
    <li><code class="nowrap">dcos-backup-master.socket</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="/1.10/administering-clusters/backup-and-restore/">文档</a></li>
    <li><a href="/1.10/administering-clusters/backup-and-restore/backup-restore-api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-gui">DC/OS GUI</h2>
<div>
<p><strong>描述:</strong> The DC/OS GUI (网页界面)是一个基于浏览器的系统仪表盘和控制中心</p>
<p><strong>系统服务:</strong> N/A - 图形界面由Admin Router提供.</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="/1.10/gui/">文档</a></li>
    <li><a href="https://github.com/dcos/dcos-ui">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-cli">DC/OS CLI</h2>
<div>
<p><strong>描述:</strong> The DC/OS CLI是基于终端的远程客户端</p>
<p><strong>系统服务:</strong> N/A - 登令行是客户下载的二进制文件.</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="/1.10/cli/">文档</a></li>
    <li><a href="https://github.com/dcos/dcos-cli">源代码</a></li>
  </ul>
</p>
</div>
</div>


# Container Orchestration

容器编排是对容器化进程及其消耗的资源进行持续的，自动化调度，协调，管理。

DC/OS包括内置的编排程序，是常用的基于容器的抽象：任务和服务。 许多案例直接由这些基本编排来完成，但是它也能为需要更灵活的程序生命周期管理的程序部署自定义任务调度器。

<div data-role="collapsible">
<h2 id="marathon">Marathon</h2>
<div>
<p><strong>描述:</strong> Marathon负责管理长期运行的服务 (apps and pods).</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-marathon.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://mesosphere.github.io/marathon/">网站</a></li>
    <li><a href="/1.10/deploying-services/">文档</a></li>
    <li><a href="https://github.com/mesosphere/marathon">源代码</a></li>
    <li><a href="/1.10/deploying-services/marathon-api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-jobs">DC/OS Jobs (Metronome)</h2>
<div>
<p><strong>描述:</strong> DC/OS Jobs (Metronome)管理短期的，计划性的或立即启用的容器化任务.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-metronome.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="/1.10/deploying-jobs/">文档</a></li>
    <li><a href="https://github.com/dcos/metronome">源代码</a></li>
    <li><a href="https://dcos.github.io/metronome/docs/generated/api.html">API参考资料</a></li>
  </ul>
</p>
</div>
</div>


# Container Runtimes

容器运行时在独立于操作系统的环境中执行和管理进程。

DC/OS支持多种容器运行时，通过[Mesos' containerizer abstraction](http://mesos.apache.org/documentation/latest/containerizers/).

<div data-role="collapsible">
<h2 id="universal-container-runtime">Universal Container Runtime</h2>
<div>
<p><strong>描述:</strong> 通用容器运行时（Mesos Containerizer）是内置于Mesos Agent中的逻辑组件，在技术上不是一个独立的进程。它使用可配置的隔离器来运行Mesos任务。通用容器运行时支持多种镜像格式，包括不使用Docker引擎的Docker镜像。</p>
<p><strong>系统服务:</strong> N/A - 通用容器运行时是Mesos Agent的一个部分.</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="http://mesos.apache.org/documentation/latest/containerizers/">Mesos容器文档Documentation</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="docker-engine">Docker Engine</h2>
<div>
<p><strong>描述:</strong> Docker引擎不是由DC/OS安装程序安装的，而是作为在每个节点上运行的系统依赖项。Mesos Agent还包含一个独立的逻辑组件，称为Docker Containerizer，它将Mesos任务的容器化委托给Docker引擎。</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">docker.service</code> - Docker引擎并不由DC/OS安装程序安装.</li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="http://mesos.apache.org/documentation/latest/docker-containerizer/">Docker容器文档</a></li>
    <li><a href="https://docs.docker.com/engine/">Docker引擎文档</a></li>
    <li><a href="https://github.com/docker/docker/">Docker引擎源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="docker-gc">Docker GC</h2>
<div>
<p><strong>描述:</strong> Docker GC定时收集Docker容器和镜像产生的垃圾.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-docker-gc.service</code></li>
    <li><code class="nowrap">dcos-docker-gc.timer</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/spotify/docker-gc">源代码</a></li>
  </ul>
</p>
</div>
</div>


#日志和性能指标

没有软件能够完美运行，特别是第一次。 在一个集群中分配任务，分析和调试这些服务能正常运行是单调乏味和痛苦的。 因此，DC/OS包括多个组件，通过聚合，缓存和流式传输日志，性能指标和集群状态元数据，来帮助缓解调试分布式系统的痛苦。

<div data-role="collapsible">
<h2 id="dcos-network-metrics">DC/OS Network Metrics <span class="badge badge--content-header badge--enterprise">ENTERPRISE</span></h2>
<div>
<p><strong>描述:</strong> DC/OS Network Metrics暴露网络相关的性能指标. DC/OS Network Metrics也被称为DC/OS网络API.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-networking_api.service</code></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-diagnostics">DC/OS Diagnostics</h2>
<div>
<p><strong>描述:</strong> DC/OS Diagnostics聚合并暴露组件健康状态. DC/OS Diagnostics也被称为DC/OS分布式分析工具.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-diagnostics.service</code></li>
    <li><code class="nowrap">dcos-diagnostics.socket</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/dcos-diagnostics">源代码</a></li>
    <li><a href="/1.10/monitoring/#system-health-http-api-endpoint">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-log">DC/OS Log</h2>
<div>
<p><strong>描述:</strong> DC/OS日志暴露节点，组件和容器(任务)日志.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-log-master.service</code></li>
    <li><code class="nowrap">dcos-log-master.socket</code></li>
    <li><code class="nowrap">dcos-log-agent.service</code></li>
    <li><code class="nowrap">dcos-log-agent.socket</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/dcos-log">源代码</a></li>
    <li><a href="/1.10/monitoring/logging/logging-api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="logrotate">Logrotate</h2>
<div>
<p><strong>描述:</strong> Logrotate管理历史日志文件的回滚，压缩和删除。</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-logrotate-master.service</code></li>
    <li><code class="nowrap">dcos-logrotate-master.timer</code></li>
    <li><code class="nowrap">dcos-logrotate-agent.service</code></li>
    <li><code class="nowrap">dcos-logrotate-agent.timer</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://linux.die.net/man/8/logrotate">文档</a></li>
    <li><a href="https://github.com/logrotate/logrotate">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-metrics">DC/OS Metrics</h2>
<div>
<p><strong>描述:</strong> DC/OS性能指标服务暴露节点，容器和应用程序的性能指标.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-metrics-master.service</code></li>
    <li><code class="nowrap">dcos-metrics-master.socket</code></li>
    <li><code class="nowrap">dcos-metrics-agent.service</code></li>
    <li><code class="nowrap">dcos-metrics-agent.socket</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/dcos-metrics">源代码</a></li>
    <li><a href="/1.10/metrics/metrics-api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-signal">DC/OS Signal</h2>
<div>
<p><strong>描述:</strong> DC/OS信号服务报告集群遥测和分析数据帮助提高DC/OS。管理员可以在安装时<a href="/1.10/installing/oss/opt-out/#telemetry">不安装遥测</a></p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-signal.service</code></li>
    <li><code class="nowrap">dcos-signal.timer</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/dcos-signal">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-history">DC/OS History</h2>
<div>
<p><strong>描述:</strong> DC/OS历史服务通过缓存和暴露系统历史状态在图形界面中展现集群使用数据.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-history.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/dcos/tree/master/packages/dcos-history/extra">源代码</a></li>
    <li><a href="https://github.com/dcos/dcos/tree/master/packages/dcos-history/extra#api">API参考资料</a></li>
  </ul>
</p>
</div>
</div>


# Networking

在这样的环境，机器被赋予数字而不是名称，任务被自动调度，依赖关系被声明性地定义，服务在分布式运行，网络管理也需要从插电缆升级到配置软件定义网络。 为实现这一目标，DC/OS包括一系列如路由，代理，域名解析，虚拟IP，负载平衡和分布式重新配置等网络组件。

<div data-role="collapsible">
<h2 id="admin-router">Admin Router</h2>
<div>
<p><strong>描述:</strong> Admin Router使用<a href="https://www.nginx.com/"> NGINX </a>为组件和服务公开一个统一的控制界面代理。Admin Router代理程序代理节点的健康状态，日志，性能指标和程序包管理内部端点。</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-adminrouter.service</code></li>
    <li><code class="nowrap">dcos-adminrouter-agent.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/adminrouter">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="mesos-dns">Mesos DNS</h2>
<div>
<p><strong>描述:</strong> Mesos域名解析服务提供集群内基于服务发现的域名解析.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-mesos-dns.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="http://mesosphere.github.io/mesos-dns/">文档</a></li>
    <li><a href="https://github.com/mesosphere/mesos-dns">源代码</a></li>
    <li><a href="/1.10/networking/mesos-dns/mesos-dns-api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dns-forwarder">DNS Forwarder (Spartan)</h2>
<div>
<p><strong>描述:</strong> DNS Forwarder (Spartan) 转发DNS请求到多台DNS服务器。当发现Spartan出现问题时，Spartan Watchdog程序重启Spartan服务.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-spartan.service</code></li>
    <li><code class="nowrap">dcos-spartan-watchdog.service</code></li>
    <li><code class="nowrap">dcos-spartan-watchdog.timer</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/spartan">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="generate-resolv.conf">Generate resolv.conf</h2>
<div>
<p><strong>描述:</strong> 生成resolv.conf，通过更新<code class="nowrap">/etc/resolv.conf</code>以完成网络名称解析，以促成DC/OS的软件定义网络.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-gen-resolvconf.service</code></li>
    <li><code class="nowrap">dcos-gen-resolvconf.timer</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/dcos/blob/master/packages/dcos-net/extra/gen_resolvconf.py">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="minuteman">Minuteman</h2>
<div>
<p><strong>描述:</strong> Minuteman提供分布式的<a href="https://en.wikipedia.org/wiki/Transport_layer">第4层</a> 虚拟IP负载均衡.</p>
<p><strong>系统服务:</strong> N/A - 包括在<a href="#navstar">Navstar</a>中.</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="/1.10/networking/load-balancing-vips/">文档</a></li>
    <li><a href="https://github.com/dcos/minuteman">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="navstar">Navstar</h2>
<div>
<p><strong>描述:</strong> Navstar使用<a href="https://en.wikipedia.org/wiki/Virtual_Extensible_LAN">VXLAN</a>管理虚拟重叠网络，并管理分布式4层虚拟IP负载均衡.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-navstar.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/navstar">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="epmd">Erlang Port Mapping Daemon (EPMD)</h2>
<div>
<p><strong>描述:</strong> Erlang Port Mapping Daemon (EPMD) 提供分布式Erlang程序间的通讯.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-epmd.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/erlang/epmd">源代码</a></li>
  </ul>
</p>
</div>
</div>


#包管理

正如主机操作系统需要软件包管理来安装，升级，配置和删除单个应用程序和服务一样，数据中心操作系统也需要软件包管理来为分布式服务提供相同的操作。 在DC/OS中，有两个级别的包管理：机器级别为组件服务; 集群级别为用户服务。

<div data-role="collapsible">
<h2 id="dcos-package-manager">DC/OS Package Manager (Cosmos)</h2>
<div>
<p><strong>描述:</strong> DC/OS Package Manager (Cosmos) 从<a href="/1.10/administering-clusters/repo/">DC/OS package repositories</a>安装和管理DC/OS软件包, 例如<a href="https://github.com/mesosphere/universe">Mesosphere Universe</a>.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-cosmos.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/cosmos">源代码</a></li>
    <li><a href="/1.10/deploying-services/package-api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-component-package-manager">DC/OS Component Package Manager (Pkgpanda)</h2>
<div>
<p><strong>描述:</strong> DC/OS Component Package Manager (Pkgpanda) 安装和管理DC/OS组件。</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-pkgpanda-api.service</code></li>
    <li><code class="nowrap">dcos-pkgpanda-api.socket</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://github.com/dcos/dcos/tree/master/pkgpanda">源代码</a></li>
    <li><a href="/1.10/administering-clusters/component-management/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>


[enterprise]
# IAM和安全
[/enterprise]

在**DC/OS Enterprise**中身份和权限管理是一个由用户，用户组和权限组成的数据库管理。外部身份认证也可以配置使用，以利用已有的数据库。权限在Admin Router的反向代理中被审查，同时也在组件级别中审查特定的操作。安全性，如SSL证书，会被安全地生成，管理，存储，并在用户服务中使用。

<div data-role="collapsible">
<h2 id="dcos-iam">DC/OS Identity and Access Manager (Bouncer) </h2>
<div>
<p><strong>描述:</strong> DC/OS Identity and Access Manager (Bouncer) 通过管理用户，用户组，服务账号，权限以及身份认证程序来控制访问DC/OS组件的权限。除了使用本地用户数据库，DC/OS IAM也可以使用如LDAP,SAML, Open ID Connect等外部身份认证服务。为更好的权限控制，其他的DC/OS组件，如Mesos，Marathon等，直接与DC/OS IAM相集成。DC/OS IAM也被称为Bouncer。</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-bouncer.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="/1.10/security/ent/">文档</a></li>
    <li><a href="/1.10/security/ent/iam-api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="cockroachdb">CockroachDB</h2>
<div>
<p><strong><em>1.10.0新加</em></strong></p>
<p><strong>描述:</strong> CockroachDB是一个建立在事务性和高度一致的键值存储上的分布式SQL数据库.</p>
<p><strong>Note:</strong> CockroachDB目前只用于<a href="#dcos-iam">DC/OS Identity and Access Manager (Bouncer)</a>.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-cockroach.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://www.cockroachlabs.com/docs/">文档</a></li>
    <li><a href="https://github.com/cockroachdb/cockroach">源代码</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-certificate-authority">DC/OS Certificate Authority</h2>
<div>
<p><strong>描述:</strong> DC/OS Certificate Authority (CA) 为通讯安全签发加密数字证书。DC/OS CA是基于Cloudflare的<a href="https://github.com/cloudflare/cfssl">Cfssl</a>建立的.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-ca.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="/1.10/security/ent/tls-ssl/">文档</a></li>
    <li><a href="/1.10/security/ent/tls-ssl/ca-api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="dcos-secrets">DC/OS Secrets</h2>
<div>
<p><strong>描述:</strong> DC/OS Secrets提供一套安全的API从Vault, 一个安全的位置中存取密钥.</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-secrets.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="/1.10/security/ent/secrets/">文档</a></li>
    <li><a href="/1.10/security/ent/secrets/secrets-api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

<div data-role="collapsible">
<h2 id="vault">Vault</h2>
<div>
<p><strong>描述:</strong> Vault一个安全管理密钥的工具。密钥就是任何你希望严格控制访问的东西，如API密钥，密码，证书等等。Vault提供一个通用界面来存取密钥，并提供严格的访问控制及记录详细的审计日志。</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-vault.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="https://www.vaultproject.io/docs/">文档</a></li>
    <li><a href="https://github.com/mesosphere/vault/">源代码</a></li>
    <li><a href="https://www.vaultproject.io/api/">API参考资料</a></li>
  </ul>
</p>
</div>
</div>

#存储

DC/OS提供了多种不同的方式来为任务提供和分配磁盘空间和卷。 其中外部永久卷，由其自己的组件管理。

<div data-role="collapsible">
<h2 id="rex-ray">REX-Ray</h2>
<div>
<p><strong>描述:</strong> REX-Ray创建，连接，挂载外部永久卷</p>
<p>
  <strong>系统服务:</strong>
  <ul>
    <li><code class="nowrap">dcos-rexray.service</code></li>
  </ul>
</p>
<p>
  <strong>See Also:</strong>
  <ul>
    <li><a href="http://rexray.readthedocs.io/">文档</a></li>
    <li><a href="https://github.com/codedellemc/rexray">源代码</a></li>
  </ul>
</p>
</div>
</div>


<!-- # Legacy Component Changes -->


#套接字和定时器

一些组件被配置为使用[systemd套接字](https://www.freedesktop.org/software/systemd/man/systemd.socket.html)，这样就可以在请求来到时按需启动它们，而不需求一直运行并消耗不必要的资源。虽然这些套接字是独立的[系统单元](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)，但它们不会被视为单独的组件。

一些组件被配置为使用[systemd定时器](https://www.freedesktop.org/software/systemd/man/systemd.timer.html)，这些定时器允许定期执行或重启。 定期执行避免了不必要的连续执行和资源消耗。定期重启允许从下游依赖关系中获取新的配置，如基于时间的DNS缓存过期。虽然这些定时器是独立的[系统单元](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)，但它们不被视为单独的组件。

#组件安装 

DC/OS组件通过[DC/OS Component Package Manager (Pkgpanda)](https://github.com/dcos/dcos/tree/master/pkgpanda)来安装，升级和管理, 系统单元的包管理器.

使用DC/OS安装程序来管理的完整软件包列表，请参阅[packages directory of the DC/OS source repository](https://github.com/dcos/dcos/tree/master/packages).


# Systemd服务

大部分的DC/OS组件以[systemd services](/1.10/overview/concepts/#systemd-service)的方式在DC/OS节点上运行.

查看某个节点上运行的systemd组件列表, 可通过列`/etc/systemd/system/dcos.target.wants/`目录内容，或是执行`systemctl | grep dcos-`来查看其状态.

##主节点

```
$ ls /etc/systemd/system/dcos.target.wants/ -1
dcos-adminrouter.service
dcos-backup-master.service
dcos-backup-master.socket
dcos-bouncer-legacy.service
dcos-bouncer.service
dcos-ca.service
dcos-cockroach.service
dcos-cosmos.service
dcos-diagnostics.service
dcos-diagnostics.socket
dcos-epmd.service
dcos-exhibitor.service
dcos-gen-resolvconf.service
dcos-gen-resolvconf.timer
dcos-history.service
dcos-log-master.service
dcos-log-master.socket
dcos-logrotate-master.service
dcos-logrotate-master.timer
dcos-marathon.service
dcos-mesos-dns.service
dcos-mesos-master.service
dcos-metrics-master.service
dcos-metrics-master.socket
dcos-metronome.service
dcos-navstar.service
dcos-networking_api.service
dcos-pkgpanda-api.service
dcos-secrets.service
dcos-secrets.socket
dcos-signal.service
dcos-signal.timer
dcos-spartan.service
dcos-spartan-watchdog.service
dcos-spartan-watchdog.timer
dcos-vault.service
```

## 私有代理节点

```
$ ls /etc/systemd/system/dcos.target.wants/ -1
dcos-adminrouter-agent.service
dcos-diagnostics.service
dcos-diagnostics.socket
dcos-docker-gc.service
dcos-docker-gc.timer
dcos-epmd.service
dcos-gen-resolvconf.service
dcos-gen-resolvconf.timer
dcos-log-agent.service
dcos-log-agent.socket
dcos-logrotate-agent.service
dcos-logrotate-agent.timer
dcos-mesos-slave.service
dcos-metrics-agent.service
dcos-metrics-agent.socket
dcos-navstar.service
dcos-pkgpanda-api.service
dcos-rexray.service
dcos-signal.timer
dcos-spartan.service
dcos-spartan-watchdog.service
dcos-spartan-watchdog.timer
```

## 公共代理节点

```
$ ls /etc/systemd/system/dcos.target.wants/ -1
dcos-adminrouter-agent.service
dcos-diagnostics.service
dcos-diagnostics.socket
dcos-docker-gc.service
dcos-docker-gc.timer
dcos-epmd.service
dcos-gen-resolvconf.service
dcos-gen-resolvconf.timer
dcos-log-agent.service
dcos-log-agent.socket
dcos-logrotate-agent.service
dcos-logrotate-agent.timer
dcos-mesos-slave-public.service
dcos-metrics-agent.service
dcos-metrics-agent.socket
dcos-navstar.service
dcos-pkgpanda-api.service
dcos-rexray.service
dcos-signal.timer
dcos-spartan.service
dcos-spartan-watchdog.service
dcos-spartan-watchdog.timer
```


# Changes Since DC/OS 1.9

- [Admin Router](#admin-router) - Admin Router现使用动态DNS解析.外部的`dcos-adminrouter-reload`服务和计时器已移除。
- [DC/OS Backup](#dcos-backup) - DC/OS备份服务和套接字是在DC/OS 1.10.0为[备份和恢复](/1.10/administering-clusters/backup-and-restore/) 功能新加入的. [enterprise type="inline" size="small" /]
- [DC/OS Component Package Manager](#dcos-component-package-manager) - 为避免DC/OS升级过程中的竞争状态, DC/OS组件包管理器套接字文件现使用[gunicorn](http://gunicorn.org/)管理，而不是systemd.
- [DC/OS Identity and Access Manager](#dcos-iam) - 为提升存储性能, DC/OS IAM现使用CockroachDB作为默认存储，而不是ZooKeeper.如要使用ZooKeeper作为存储, 可以使用`dcos-bouncer-legacy`服务. [enterprise type="inline" size="small" /]
- [CockroachDB](#cockroachdb) - CockroachDB作为DC/OS IAM存储在DC/OS 1.10.0中加入. [enterprise type="inline" size="small" /]
- [DC/OS Secrets](#dcos-secrets) - 为提高安全性，DC/OS Secrets现使用套接字，而不是端口. [enterprise type="inline" size="small" /]
