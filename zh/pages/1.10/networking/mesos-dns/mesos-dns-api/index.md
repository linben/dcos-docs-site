---
layout: layout.pug
navigationTitle: Mesos DNS API
title: Mesos DNS API
menuWeight: 201
excerpt: ""
enterprise: true
---
您可以使用Mesos DNS API来发现其他应用程序的IP地址和端口。

# 路线

访问Mesos DNS API通过每个节点上的管理路由器使用以下路由进行代理：

```bash
curl -H "Authorization: token=<auth-token>" http://<public-master-ip>/mesos_dns/v1/
```

访问代理节点的Mesos DNS API也通过主节点进行代理：

```bash
curl -H "Authorization: token=<auth-token>" http://<public-master-ip>/system/v1/agent/{agent_id}/mesos_dns/v1/
```

# 格式

Mesos DNS API请求和响应正文使用JSON格式。

请求必须包括接受标头:

    Accept: application/json
    

响应将包括内容类型标头:

    Content-Type: application/json
    

# 授权 (仅限企业)

所有 Mesos 的 DNS API 路由都需要使用身份验证。

要验证API请求，请参阅[获取身份验证令牌](/1.10/security/ent/iam-api/#obtaining-an-authentication-token)和[传递验证令牌](/1.10/security/ent/iam-api/#passing-an-authentication-token)。

Mesos DNS API 还需要通过以下权限进行授权:

| 路径                                          | 权限                               |
| ------------------------------------------- | -------------------------------- |
| `/system/mesos_dns/v1/`                     | `dcos:adminrouter:ops:mesos-dns` |
| `/system/v1/agent/{agent_id}/mesos_dns/v1/` | `dcos:adminrouter:system:agent`  |

使用 ` dcos:superuser` 权限的用户也可以访问所有路由。

要为您的帐户分配权限, 请参阅 [ 权限参考 ](/1.10/security/ent/perms-reference/)。

# 资源

Mesos-DNS 实现了一个简单的 REST API, 用于通过 HTTP 进行服务发现。这些示例假定您具有到该节点的 [ SSH 连接 ](/1.10/administering-clusters/sshcluster/)。

## <a name="get-version"></a>GET /v1/version

Lists in JSON format the Mesos-DNS version and source code URL.

```bash
curl -H "Authorization: token=<auth-token>" http://<public-master-ip>/mesos_dns/v1/version
```

输出会是这样：

```json
{
  "Service": "Mesos-DNS",
  "URL": "https://github.com/mesosphere/mesos-dns",
  "Version": "dev"
 }
```

## <a name="get-config"></a>GET /v1/config

以JSON格式列出Mesos-DNS配置参数。

```bash
curl -H "Authorization: token=<auth-token>" http://<public-master-ip>/mesos_dns/v1/config
```

DC / OS开源的输出应该类似于：

```json
{
  "RefreshSeconds": 30,
  "Port": 61053,
  "Timeout": 5,
  "StateTimeoutSeconds": 300,
  "ZkDetectionTimeout": 30,
  "HttpPort": 8123,
  "TTL": 60,
  "SOASerial": 1495828250,
  "SOARefresh": 60,
  "SOARetry": 600,
  "SOAExpire": 86400,
  "SOAMinttl": 60,
  "SOAMname": "ns1.mesos.",
  "SOARname": "root.ns1.mesos.",
  "Masters": null,
  "ZoneResolvers": {},
  "Resolvers": [
   "169.254.169.253"
  ],
  "IPSources": [
   "host",
   "netinfo"
  ],
  "Zk": "zk://zk-1.zk:2181,zk-2.zk:2181,zk-3.zk:2181,zk-4.zk:2181,zk-5.zk:2181/mesos",
  "Domain": "mesos",
  "File": "/opt/mesosphere/etc/mesos-dns.json",
  "Listener": "0.0.0.0",
  "HTTPListener": "0.0.0.0",
  "RecurseOn": true,
  "DnsOn": true,
  "HttpOn": true,
  "ExternalOn": true,
  "EnforceRFC952": false,
  "SetTruncateBit": false,
  "EnumerationOn": true,
  "MesosHTTPSOn": false,
  "CACertFile": "",
  "CertFile": "",
  "KeyFile": "",
  "MesosCredentials": {
   "Principal": "",
   "Secret": ""
  },
  "IAMConfigFile": "",
  "MesosAuthentication": ""
 }
```

企业DC / OS的输出应该类似于：

```json
{
  "RefreshSeconds": 30,
  "Port": 61053,
  "Timeout": 5,
  "StateTimeoutSeconds": 300,
  "ZkDetectionTimeout": 30,
  "HttpPort": 8123,
  "TTL": 60,
  "SOASerial": 1495828138,
  "SOARefresh": 60,
  "SOARetry": 600,
  "SOAExpire": 86400,
  "SOAMinttl": 60,
  "SOAMname": "ns1.mesos.",
  "SOARname": "root.ns1.mesos.",
  "Masters": null,
  "ZoneResolvers": {},
  "Resolvers": [
   "169.254.169.253"
  ],
  "IPSources": [
   "host",
   "netinfo"
  ],
  "Zk": "zk://zk-1.zk:2181,zk-2.zk:2181,zk-3.zk:2181,zk-4.zk:2181,zk-5.zk:2181/mesos",
  "Domain": "mesos",
  "File": "/opt/mesosphere/etc/mesos-dns-enterprise.json",
  "Listener": "0.0.0.0",
  "HTTPListener": "127.0.0.1",
  "RecurseOn": true,
  "DnsOn": true,
  "HttpOn": true,
  "ExternalOn": true,
  "EnforceRFC952": false,
  "SetTruncateBit": false,
  "EnumerationOn": true,
  "MesosHTTPSOn": true,
  "CACertFile": "/run/dcos/pki/CA/certs/ca.crt",
  "CertFile": "/run/dcos/pki/tls/certs/mesos-dns.crt",
  "KeyFile": "/run/dcos/pki/tls/private/mesos-dns.key",
  "MesosCredentials": {
   "Principal": "",
   "Secret": ""
  },
  "IAMConfigFile": "/run/dcos/etc/mesos-dns/iam.json",
  "MesosAuthentication": "iam"
 }
```

## <a name="get-hosts"></a>GET /v1/hosts/{host}

以JSON格式列出与主机名对应的IP地址。 它相当于DNS A记录查找。

**注意：** HTTP接口仅解析Mesos域中的主机名。

```bash
curl -H "Authorization: token=<auth-token>" http://<public-master-ip>/mesos_dns/v1/hosts/nginx.marathon.mesos
```

输出会是这样：

```json
[
    {"host":"nginx.marathon.mesos.","ip":"10.249.219.155"},
    {"host":"nginx.marathon.mesos.","ip":"10.190.238.173"},
    {"host":"nginx.marathon.mesos.","ip":"10.156.230.230"}
]
```

## <a name="get-service"></a>GET /v1/services/{service}

以JSON格式列出与主机名对应的主机名，IP地址和端口。 这相当于DNS SRV记录查找。

**注意：** HTTP接口仅解析Mesos域中的服务名称。

```bash
curl -H "Authorization: token=<auth-token>" http://<public-master-ip>/mesos_dns/v1/services/_nginx._tcp.marathon.mesos
```

输出会是这样：

```json
[
    {"host":"nginx-s2.marathon.mesos.","ip":"10.249.219.155","port":"31644","service":"_nginx._tcp.marathon.mesos."},
    {"host":"nginx-s1.marathon.mesos.","ip":"10.190.238.173","port":"31667","service":"_nginx._tcp.marathon.mesos."},
    {"host":"nginx-s0.marathon.mesos.","ip":"10.156.230.230","port":"31880","service":"_nginx._tcp.marathon.mesos."}
]
```