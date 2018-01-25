---
layout: layout.pug
navigationTitle: API 版本
title: API 版本
menuWeight: 2
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

The DC/OS API is backed by many loosely coupled components; some are standalone projects and others are designed exclusively for DC/OS. 因此, 支持多种版本控制机制: 组件、路由和资源版本控制。

若要了解如何制定特定的 api 调用, 请参阅该路由的组件 api 参考文档。

# 组件丢失

具有自己的开源社区的组件 (如 Mesos、马拉松和 Mesos DNS) 具有基于其众所周知的组件名称的路由。 这些路由将版本控制委托给后端组件服务。

For example, the [Marathon component](/1.10/overview/architecture/components/#marathon) serves the [Marathon API](/1.10/deploying-services/marathon-api/) under the route `/service/marathon` and one of its resource paths is `/v2/apps`, so the full path to that resource is `/service/marathon/v2/apps`.

# Route Versioning

Components that have been specifically designed for DC/OS generally follow another versioning pattern, where the name of the component takes a back seat to the name of the feature set. These routes often include a version to make it easier to support renaming or replacing components over time.

For example, the [DC/OS Diagnostics component](/1.10/overview/architecture/components/#dcos-diagnostics) serves the [System Health API](/1.10/monitoring/#system-health-http-api-endpoint) under the route `/system/health/v1` and one of its resource paths is `/report`, so the full path to that resource is `/system/health/v1/report`.

# Resource Versioning

Some components avoid path versioning altogether and use content negotiation at the resource level to simultaneously support multiple versions of an API at the same path.

For example, the [DC/OS Package Manager (Cosmos) component](/1.10/overview/architecture/components/#dcos-package-manager) serves the [Package API](/1.10/deploying-services/package-api/) under the route `/package` and one of its resource paths is `/list`, so the full path to that resource is `/package/list`. The version of the request and desired version of the response are specified respectively by the `Content-Type` and `Accept` HTTP headers:

    Content-Type: application/vnd.dcos.package.list-request+json;charset=utf-8;version=v1
    Accept:       application/vnd.dcos.package.list-response+json;charset=utf-8;version=v1