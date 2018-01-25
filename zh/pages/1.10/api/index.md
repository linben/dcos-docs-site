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

管理路由器公开几种类型的路由:

- ** 代理路由 ** 从另一个 URL 检索资源。
- ** 文件路由 ** 检索静态文件。
- ** lua 路由 ** 执行 lua 代码以生成响应。
- ** 重定向路由 ** 重定向到另一个 URL。
- ** 重写路由 ** 将路由转换为其他路由。

# 族类排料

要确定群集的 URL, 请参阅 [ 群集访问 ](/1.10/api/access/)。

# 版本

DC / OS API的部分由组件，路由或资源进行版本控制。

有关版本控制机制的详细信息，请参阅[版本控制](/1.10/api/versioning/)。

# 验证

某些路由未经身份验证，但大多数都需要身份验证令牌。

有关如何获取和使用身份验证令牌的详细信息, 请参阅 [ 身份验证 HTTP api 终结点 ](/1.10/security/ent/iam-api/)。

# 身份验证

大多数认证的路由也需要通过权限进行授权。 DC / OS Enterprise中的权限由分层资源标识符和操作(create, read, update, full) 组成。

许可执行可以在两个级别执行。

- **粗粒度权限**在路由级别由管理路由器[执行](/1.10/security/ent/perms-reference/#admin-router)。
- ** 细粒度权限 ** 由各个后端组件服务强制执行。

[权限管理](/1.10/security/ent/perms-management/)可由用户使用<a href =“/1.10/security/ent/perms-reference/#superuser”>超级用户权限</a>，使用[身份和访问管理API ](/1.10/security/ent/iam-api/)。 具有超级用户权限的使用者也具有访问所有路由的隐式权限。

# 路线使用

- 若要通过 ** 代理路由 ** 确定 API 资源的完整 url, 请加入群集 url、路由和后端组件资源路径。
    
        <cluster-url>/<route>/<resource-path>
        
    
    例如, 获取 Mesos 版本来自: ` https://dcos.example.com/mesos/version `

- ** 文件路由 ** 没有后端组件, 但可以提供文件目录或单个文件。因此, 对于文件路由, 请指定文件路径而不是后端组件资源路径。
    
        <cluster-url>/<route>/<file-path>
        
    
    例如, 从以下内容获取群集的 DC/OS 版本: ` https://dcos.example.com/dcos-metadata/dcos-version.json `

- ** Lua 路由 ** 立即在管理路由器中执行代码, 而不代理到外部后端组件。因此, 对于 Lua 路由, 路由后不需要任何路径。
    
        <cluster-url>/<route>
        
    
    例如，从` https://dcos.example.com/metadata `获取主节点和群集ID的公共IP

- ** 重写和重定向路由 ** 可能会在返回资源之前通过一个或多个其他 url 或路由。 因此, 对于这些路由, 请按照 url 和路由链来查找端点。 资源路径将取决于最终端点。
    
    大多数重写和重定向都在另一个DC / OS API路由中终止，其中明显的例外是`/login`，它使用OpenID Connect与外部身份提供者进行授权，然后重定向回DC / OS API。