---
layout: layout.pug
navigationTitle: Securing a Cluster
excerpt: ""
title: Securing a Cluster
menuWeight: 7
---
This topic discusses the security features in DC/OS and best practices for deploying DC/OS securely.

## General security concepts

DC/OS is based on a Linux kernel and userspace. The same best practices for securing any Linux system apply to securing DC/OS, including setting correct file permissions, restricting root and normal user accounts, protecting network interfaces with iptables or other firewalls, and regularly applying updates from the Linux distribution used with DC/OS to ensure that system libraries, utilities and core services like systemd and OpenSSH are secure.

## Security Zones

At the highest level we can distinguish three security zones in a DC/OS deployment, namely the admin, private, and public security zones.

### Admin zone

The **admin** zone is accessible via HTTP/HTTPS and SSH connections, and provides access to the master nodes. It also provides reverse proxy access to the other nodes in the cluster via URL routing. For security, the DC/OS cloud template allows configuring a whitelist so that only specific IP address ranges are permitted to access the admin zone.

#### Steps for Securing Admin Router

By default, Admin Router will permit unencrypted HTTP traffic. This is not considered secure, and you must provide a valid TLS certificate and redirect all HTTP traffic to HTTPS to properly secure access to your cluster.

After you have a valid TLS certificate, install the certificate on each master. Copy the certificate and private key to a well known location, such as under `/etc/ssl/certs`.

If you run HAProxy in front of Admin Router, you should secure the communication between them. For information about securing your communication, see the [documentation](/1.10/security/oss/tls-ssl/haproxy-adminrouter/).

### Private zone

The **private** zone is a non-routable network that is only accessible from the admin zone or through the edge router from the public zone. Deployed services are run in the private zone. This zone is where the majority of agent nodes are run.

### Public zone

The optional **public** zone is where publicly accessible applications are run. 通常, 此区域中只运行少量的代理节点。 边缘路由器将通信转发到在专用区域中运行的应用程序。

公共区域中的代理节点被标记为具有特殊的角色, 因此只能在此安排特定任务。 这些代理节点具有公共和专用 IP 地址, 并且只有特定端口在其 iptables 防火墙中打开。

By default, when using the cloud-based installers such as the AWS CloudFormation templates, a large number of ports are exposed to the Internet for the public zone. 在生产系统中, 您不太可能公开所有这些端口。 建议您关闭除80和443之外的所有端口 (用于 HTTP/https 通信), 并使用 [ 马拉松 lb ](/services/marathon-lb/) 和 HTTPS 来管理入口通信。

### 典型 AWS 部署

A typical AWS deployment including AWS Load Balancers is shown below:

![Security Zones](/1.10/img/security-zones.jpg)

## Admin Router

Access to the admin zone is controlled by the Admin Router.

HTTP requests incoming to your DC/OS cluster are proxied through the Admin Router (using [Nginx](http://nginx.org) with [OpenResty](https://openresty.org) at its core). The Admin Router denies access to most HTTP endpoints for unauthenticated requests. In order for a request to be authenticated, it needs to present a valid authentication token in its Authorization header. A token can be obtained by going through the authentication flow, as described in the next section.

Authenticated users are authorized to perform arbitrary actions in their cluster. That is, there is currently no fine-grained access control in DC/OS besides having access or not having access to services.

See the [Security Administrator's Guide](/1.10/security/) for more information.