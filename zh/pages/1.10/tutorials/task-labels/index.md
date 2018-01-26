---
layout: layout.pug
navigationTitle: Labeling Tasks and Jobs
title: Labeling Tasks and Jobs
menuWeight: 6
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

本教程将演示如何使用DC / OS Web界面和Marathon HTTP API定义标签，以及如何根据标签值标准查询正在运行的应用程序和作业的相关信息。

在DC / OS群集中部署应用程序，容器或作业时，可以将标记或标签与部署的组件关联，以跟踪和报告这些组件的使用情况。 在DC / OS群集中部署应用程序，容器或作业时，可以将标记或标签与部署的组件关联，以跟踪和报告这些组件的使用情况。...

# 为应用程序和任务分配标签

您可以通过DC / OS Web界面的**Services**选项卡或从DC / OS CLI将标签附加到任务。 您可以指定多个标签，但每个标签只能有一个值。

## 从DC / OS Web界面为应用程序或任务分配标签

在DC / OS Web界面中，点击**Services**标签。 您可以在部署新服务时添加标签，也可以从**Labels**标签中编辑现有标签。

## 从DC / OS CLI为应用程序或任务分配一个标签

您也可以在应用程序定义的` labels `参数中指定标签值。

    vi myapp.json
    
    {
        "id": "myapp",
        "cpus": 0.1,
        "mem": 16.0,
        "ports": [
            0
        ],
        "cmd": "/opt/mesosphere/bin/python3 -m http.server $PORT0",
        "instances": 2,
        "labels": {
            "COST_CENTER": "0001"
        }
    }
    

然后，从DC / OS CLI进行部署：

```bash
dcos marathon app add <myapp>.json
```

# 为作业分配标签

您可以通过DC / OS Web界面的**Jobs**选项卡或从DC / OS CLI将作业标签附加到作业中。 您可以指定多个标签，但每个标签只能有一个值。

## 从DC / OS Web界面为作业分配一个标签

在DC / OS网络界面中，点击**Jobs**标签。 您可以在部署新作业时添加标签，也可以从**Labels**标签中编辑现有标签。

![工作标签](/1.10/img/job-label.png)

## 从DC / OS CLI为作业分配一个标签

您还可以在作业定义的` labels `参数中指定标签值。

    vi myjob.json
    
     ```json
        {
          "id": "my-job",
          "description": "A job that sleeps",
          "labels": {
            "department": "marketing"
          },
          "run": {
            "cmd": "sleep 1000",
            "cpus": 0.01,
            "mem": 32,
            "disk": 0
          }
        }
     ```
    

然后，从DC / OS CLI进行部署：

```bash
dcos job add <myjob>.json
```

# 显示标签信息

部署和启动应用程序后，可以按照DC / OS UI的**Services**选项卡中的标签进行过滤。

您还可以使用DC / OS CLI中的Marathon HTTP API根据标签值标准查询正在运行的应用程序。

下面的代码片段显示了发给Marathon HTTP API的HTTP请求。 本示例中使用curl程序提交HTTP GET请求，但可以使用任何能够发送HTTP GET / PUT / DELETE请求的程序。 您可以看到HTTP端点是` https://52.88.210.228/marathon/v2/apps `，并且与HTTP请求一起发送的参数包括标签条件`？label = COST_CENTER ==0001`：

    curl --insecure \
    > https://52.88.210.228/marathon/v2/apps?label=COST_CENTER==0001 \
    > | python -m json.tool | more
    

您也可以指定多个标签条件，如下所示：`？label = COST_CENTER == 0001，COST_CENTER == 0002 `

在上面的示例中，您收到的响应将仅包含具有`0001> `值定义的标签` COST_CENTER `的应用程序。 资源指标也包括在内，如CPU份额和分配的内存量。 在响应的底部，您可以看到此应用程序的部署日期/时间，可用于计算计费或退款的正常运行时间。