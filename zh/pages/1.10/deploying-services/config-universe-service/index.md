---
layout: layout.pug
navigationTitle: 配置宇宙服务
title: 配置宇宙服务
menuWeight: 2
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

每个宇宙服务都安装一组默认参数。您可以发现默认参数并按需要更改它们。

本主题介绍如何使用 DC/OS CLI 来配置服务。还可以使用 DC/OS UI 中的 [ ** 服务 "** ](/1.10/gui/services/) 选项卡自定义服务。

1. 使用 ` dcos 包描述-配置 < 包-名称 > ` 命令查看服务的可用配置选项。
    
    ```bash
dcos package describe --config marathon
```

输出应该是这样的：

```json
{
...
  "service": {
    "additionalProperties": false,
    "description": "Marathon app configuration properties.",
    "properties": {
      "cpus": {
        "default": 2,
        "description": "CPU shares to allocate to each Marathon instance.",
        "minimum": 0,
        "type": "number"
      },
      ...
      "instances": {
        "default": 1,
        "description": "Number of Marathon instances to run.",
        "minimum": 0,
        "type": "integer"
      },
      "mem": {
        "default": 1536,
        "description": "Memory (MB) to allocate to each Marathon instance.",
        "minimum": 512,
        "type": "number"
      }
    },
    ...
  }
}
```

2. 创建一个JSON配置文件。 您可以选择一个任意名称，但是您可能想要选择一个模式，如`< package-name> -config.json `。 例如，` marathon-config.json `。
    
    ```bash
nano marathon-config.json
```

3. 使用` properties `对象来构建您的JSON选项文件。 例如，要将马拉松CPU份额的数量更改为3，并将内存分配更改为2048：
    
    ```json
{
  "application": {
    "cpus": 3.0, "mem": 2048.0
   }
}
```

4. 在DC / OS CLI中，使用指定的自定义选项文件安装DC / OS服务：
    
    ```bash
dcos package install --options=marathon-config.json marathon
```

有关更多信息，请参阅[ dcos包](/1.10/cli/command-reference/dcos-package)文档。