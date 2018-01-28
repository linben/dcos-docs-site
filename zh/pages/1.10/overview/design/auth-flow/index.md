---
layout: layout.pug
navigationTitle:  Authentication Architecture
excerpt:
title: Authentication Architecture
menuWeight: 1
---


通过DC/OS用户界面完成一次认证的过程如下:

1. 用户在浏览器打开集群首页.
2. 如果用户已经有[authentication token](/1.10/security/oss/managing-authentication#log-in-cli) cookie (Admin Router负责检查)用户可以直接打开集群首页。如果没有，会重定向打开登录页面.
3. DC/OS的用户界面将`dcos.auth0.com`的登录页面加载到登录页面的iframe中, 用户在此可选择身份认证提供商，包括Google, Github,和微软帐户。
4. 用户选择身份认证提供商，在弹出窗口中完成OAuth协议流程后，为用户返回RS256-signed JRW. 得到密匙，根据标准`exp`声明有效期为5天.
5. 登录页面向Admin Router端点`http://<master-host-name>/acs/api/v1/auth/login`发送带有用户密匙的请求，该端点将其转发到[dcos-oauth](https://github.com/dcos/dcos-oauth) 服务。如果用户是第一个访问集群的用户，则会自动创建一个帐户。任何后续用户都必须由[User Management](/1.10/security/oss/user-management/) 页面中所述的集群中其他任何用户添加。如果登录到集群的用户被确定为有效，那么他们将获得HS256-signed JWT，其中包含特定于其登录的集群的`uid`声明。

为了让dcos-oauth服务验证在用户登录时收到的密匙，它需要通过HTTPS访问dcos.auth0.com获取所需的公钥。目前不支持使用代理来转发此请求。

使用HS256算法上签署集群特定标记的共享密匙在集群启动期间生成，并存储在每个主节点上的`/var/lib/dcos/auth-token-secret`和每个ZooKeeper znode上的 `/dcos/auth-token-secret`。

如上所述，为了简化设置过程，DC/OS会自动将登录的第一个用户添加到DC/OS集群。确保限制网络访问集群，直到配置第一个用户。未来的版本将允许用户在安装时进行配置。

请务必保护身份验证密匙，因为未经授权的第三方可能会使用它们登录您的集群。单个令牌无效当前不支持。如果令牌泄露，建议将受影响的用户从集群中删除。

[JWT.IO](https://jwt.io) 服务可用于解码JWTs来查看其中的内容.
