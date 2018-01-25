---
layout: layout.pug
navigationTitle: API 引用
title: API 引用
menuWeight: 150
excerpt: ""
enterprise: false
---
DC / OS API是由[ DC / OS组件](/1.10/overview/architecture/components/)支持的路由的集合，可通过称为[管理路由器](/1.10/overview/architecture/components/#admin-router)的API网关提供。

<!-- Use html img for horizontal centering -->

<img src="/1.10/img/dcos-api-routing.png" alt="DC/OS API Routing" style="display:block;margin:0 auto" />

# API网关

Admin Router是建立在NGINX之上的API网关，具有以下目标：

- 为DC / OS API提供统一的控制平面
- 代理API请求到主节点和代理节点上的组件服务
- 获取用户身份验证
- 提供DC / OS GUI

Admin Router以两种配置之一在每个DC / OS节点上运行：

- **Admin Router Master**暴露[主路线](/1.10/api/master-routes/)。
    
    此配置在每个主节点上运行，并充当用于与DC / OS组件进行交互的主API网关。

- **Admin Router Agent**公开了[agent 路线](/1.10/api/agent-routes/)。
    
    此配置在每个代理节点上运行，并提供用于监视，调试和管理的路由。
    
    一些代理路由（如日志和指标）通过主管理路由器进行代理，以允许外部访问。 其他路线，如组件管理，仅供内部使用。

# 路线类型

Admin Router exposes several types of routes:

- **Proxy Routes** retrieve resources from another URL.
- **File Routes** retrieve static files.
- **Lua Routes** execute Lua code to generate responses.
- **Redirect Routes** redirect to another URL.
- **Rewrite Routes** translate routes into other routes.

# 族类排料

要确定群集的 URL, 请参阅 [ 群集访问 ](/1.10/api/access/)。

# 版本

DC / OS API的部分由组件，路由或资源进行版本控制。

有关版本控制机制的详细信息，请参阅[版本控制](/1.10/api/versioning/)。

# 验证

某些路由未经身份验证，但大多数都需要身份验证令牌。

For details on how to obtain and use an authentication token, see [Authentication HTTP API Endpoint](/1.10/security/ent/iam-api/).

# 身份验证

大多数认证的路由也需要通过权限进行授权。 DC / OS Enterprise中的权限由分层资源标识符和操作(create, read, update, full) 组成。

Permission enforcement can be performed at two levels.

- **Course-grained permissions** are [enforced by Admin Router](/1.10/security/ent/perms-reference/#admin-router) at the route level.
- **Fine-grained permissions** are enforced by individual backend component services.

[Permissions Management](/1.10/security/ent/perms-management/) can be performed by users with the [Superuser permission](/1.10/security/ent/perms-reference/#superuser) using the [Identity and Access Management API](/1.10/security/ent/iam-api/). Users with the Superuser permission also have implicit permission to access all routes.

# Route Usage

- To determine the full URL of a API resource through a **proxy route**, join the cluster URL, route, and backend component resource path.
    
        <cluster-url>/<route>/<resource-path>
        
    
    For example, get the Mesos version from: `https://dcos.example.com/mesos/version`

- **File routes** have no backend component, but may serve a directory of files or a single file. So for file routes, specify the file path instead of the backend component resource path.
    
        <cluster-url>/<route>/<file-path>
        
    
    For example, get the DC/OS version of the cluster from: `https://dcos.example.com/dcos-metadata/dcos-version.json`

- **Lua routes** immediately execute code in Admin Router without proxying to an external backend component. So for Lua routes, no path is required after the route.
    
        <cluster-url>/<route>
        
    
    For example, get the public IP of the master node and cluster ID from: `https://dcos.example.com/metadata`

- **Rewrite and redirect routes** may pass through one or more other URLs or routes before returning a resource. So for those routes, follow the chain of URLs and routes to find the endpoint. The resource path will depend on the final endpoint.
    
    Most rewrites and redirects terminate in another DC/OS API route, with the notable exception of `/login`, which uses OpenID Connect to authorize with an external identity provider and then redirects back to the DC/OS API.