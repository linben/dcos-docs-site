---
layout: layout.pug
0: ""
title: Configuring services and pods to use secrets
menuWeight: 1
excerpt: ""
enterprise: true
---
您可以在服务定义中以环境变量或文件的方式引用密钥.

## 基于文件的密钥

您可以将密码作为文件来引用，以提高进程的安全性，或者服务从挂载在容器中的文件中读取密钥。

引用基于文件的密钥对于以下场景特别有用：

- Kerberos密钥凭据或其他证书文件。
- SSL证书。
- 包含有敏感数据的配置文件。

基于文件的密钥在可以在任务sandbox找到（`$MESOS_SANDBOX/<configured-path>`）。

**先决条件:**

- 已有的密钥。 以下示例使用存储在`developer`路径下名为`my-secret`的密钥。 如果您已完成[创建密钥](/1.10/security/ent/secrets/create-secrets/)中的步骤，将具备此先决条件。

- [安装DC/OS CLI](/1.10/cli/install/)并[安装DC/OS Enterprise CLI](/1.10/cli/enterprise-cli/#ent-cli-install)。

- 如果您的安全模式是宽容或严格的，那么在发布本节中的curl命令之前，您必须获得root证书。 如果您的安全模式被禁用，您必须从命令中删除--cacert dcos-ca.crt，然后再发出它们。

- 如果您的[安全模式](/1.10/installing/ent/custom/configuration/configuration-parameters/#security-enterprise) 是`permissive`或`strict`，在使用本节的curl命令前，需要先执行[获取DC/OS权威公钥包](/1.10/security/ent/tls-ssl/get-cert/) 中所列步骤. 如果您的[安全模式](/1.10/installing/ent/custom/configuration/configuration-parameters/#security-enterprise) 是`disabled`, 您需要从以下命令中删除`--cacert dcos-ca.crt`.

- 适合[安全模式](/1.10/security/ent/#security-modes)的权限。

<table class="table">
<tr>
<th>
  权限
</th>

<th>
  作用于
</th>
</tr>

<tr>
<td>
  <code>dcos:adminrouter:service:marathon full</code>
</td>

<td>
  所用安全模式
</td>
</tr>

<tr>
<td>
    <code>dcos:service:marathon:marathon:services:/[&lt;i>service-group&lt;/i> full</code>
</td>

<td>
  <code>strict</code>和<code>permissive</code> 安全模式
</td>
</tr>
</table>

在`strict`安全械下，用户也需要以下权限.

- `dcos:adminrouter:ops:mesos full`: 来查看 **任务** 面板信息.
- `dcos:adminrouter:ops:slave full`: 查看任务详细信息，包括日志.
    
    只要密钥路径和组路径[相匹配](/1.10//security/ent/#spaces)，服务就能够访问密钥值。

根据是否要将密钥提供给[豆荚](/1.10/deploying-services/pods/)或单个服务，过程会有所不同。

- [单个服务](#service)
- [豆荚](#pod)

# <a name="service"></a>配置单个服务使用密钥

此过程在不同的接口下会有不同。请参考您使用的接口相应段落。

- [用户图形界面](#deploying-the-service-via-the-web-interface)

- [Marathon API](#deploying-the-service-via-marathon-app-definition)

## <a name="deploying-the-service-via-the-web-interface"></a>通过用户图形界面配置单个服务使用密钥

1. 使用[上节](#service)讨论过的具备相应权限的用户登录用户图形界面.

2. 点击 **服务列表** 标签.

3. 点击右上角的 **+** 图标.
    
    ![Add a Service](/1.10/img/add-service.png)

4. 点击 **JSON Editor** 开关.

5. 选择默认JSON框架里的内容，并删除，在编辑框显示为空白，没有内容.

6. 拷贝下列之一的应用程序定义文件范例，粘贴在空白编辑框。应用程序定义文件会创建一个在develop组中新服务，引用保存在developer路径下的密钥.
    
    基于环境变量的密钥:
    
```json
{  
  "id":"/developer/service",
  "cmd":"sleep 100",
  "env":{  
     "MY_SECRET":{  
        "secret":"secret0"
     }
  },
  "secrets":{  
     "secret0":{  
        "source":"developer/my-secret"
     }
  }
}
```

在上例中，DC/OS在`"MY_SECRET"`环境变量中保存密钥。观察如何使用`"env"`和`"secrets"`对象来定义基于环境变量的密钥。

基于文件密钥:

```json
{
 "id": "developer/service",
 "cmd": "sleep 100",
 "container": {
   "volumes": [
     {
       "containerPath": "path",
       "secret": "secretpassword"
     }
   ]
 },
 "secrets": {
   "secretpassword": {
     "source": "developer/databasepassword"
   }
 }
}
```

在上例中，密钥文件名为`path`，并保存在任务sandbox(`$MESOS_SANDBOX/path`).

由于服务与密钥路径相匹配，服务可以访问密钥。更多有关路径的信息，请参看[空间](/1.10/security/ent/#spaces).

7. 点击 **REVIEW & RUN**.

8. 点击 **RUN SERVICE**.

9. 点击您的服务组名, 例如 **developer**.

10. 点击您的服务名.

11. 点击任务名.

12. 在**Details** 标签中找到您的 `DCOS_SECRETS_DIRECTIVE`.

# <a name="deploying-the-service-via-marathon-app-definition"></a>通过Marathon应用程序定义配置服务使用基于环境变量的密钥

1. 使用具务具备足够权限的用户通过`dcos auth login`登陆命令行界面. 参阅[配置服务和豆荚使用密钥](#service) 了解需要的权限.

2. 在文件编辑器内，为Marathon服务创建应用程序定义。以下应用程序定义在developer组中创建一个新服务，引用develop路径下的密钥。
    
    基于环境变量的密钥:
    
```json
{  
  "id":"/developer/service",
  "cmd":"sleep 100",
  "env":{  
     "MY_SECRET":{  
        "secret":"secret0"
     }
  },
  "secrets":{  
     "secret0":{  
        "source":"developer/my-secret"
     }
  }
}
```

在上例中，DC/OS在`"MY_SECRET"`环境变量中保存密钥。观察如何使用`"env"`和`"secrets"`对象来定义基于环境变量的密钥。

基于文件的密钥:

```json
{
 "id": "developer/service",
 "cmd": "sleep 100",
 "container": {
   "volumes": [
     {
       "containerPath": "path",
       "secret": "secretpassword"
     }
   ]
 },
 "secrets": {
   "secretpassword": {
     "source": "developer/databasepassword"
   }
 }
}
```

由于服务与密钥路径相匹配，服务可以访问密钥。更多有关路径的信息，请参看[空间](/1.10/security/ent/#spaces).

3. 将文件保存为一个能说明意义的名字，例如`myservice.json`.

4. 通过DC/OS命令添加服务到DC/OS中.
    
```bash
dcos marathon app add myservice.json
```

或者使用Marathon API来部署应用程序，如下.

```bash
curl -X POST --cacert dcos-ca.crt $(dcos config show core.dcos_url)/service/marathon/v2/apps -d @myservice.json -H "Content-type: application/json" -H "Authorization: token=$(dcos config show core.dcos_acs_token)"
```

6. 点击您的服务组名, 例如 **developer**.

7. 点击您的服务名.

8. 点击任务名.

9. 在**Details** 标签中找到您的 `DCOS_SECRETS_DIRECTIVE`.

# <a name="pod"></a>配置豆荚使用密钥

1. 使用具务具备足够权限的用户通过`dcos auth login`登陆命令行界面. 参阅[配置服务和豆荚使用密钥](#service) 了解需要的权限.

2. 在文件编辑器内，为豆荚创建应用程序定义。您可以使用`"environment"`和`"secrets"`对象添加密钥，如下例所示。以下应用程序定义在developer组中创建一个新服务，引用develop路径下的密钥。它在环境变量`"MY_SECRET"`下储存密钥.
    
    基于环境变量的密钥:
    
```json
{
  "id": "/developer/pod-secret",
  "environment": {
    "MY_SECRET": {
      "secret": "secret0"
    }
  },
  "secrets": {
    "secret0": { "source": "developer/my-secret"}
  },
  "containers": [
    {
      "name": "container-1",
      "resources": {
        "cpus": 0.1,
        "mem": 128
      },
      "exec": {
        "command": {
          "shell": "sleep 3600"
        }
      }
    }
  ],
  "scaling": {
    "kind": "fixed",
    "instances": 1
  },
  "networks": [
    {
      "mode": "host"
    }
  ]
}
```

基于文件的密钥:

```json
{
  "id": "developer/pod-with-secrets",
  "containers": [
     {
       "name": "container-1",
       "exec": {
         "command": {
           "shell": "sleep 1"
         }
     },
     "volumeMounts": [
       {
         "name": "secretvolume",
         "mountPath": "path/to/db/password"
       }
     ]
   }
 ],
 "volumes": [
   {
     "name": "secretvolume",
     "secret": "secretpassword"
   }
 ],
 "secrets": {
   "secretpassword": {
     "source": "developer/databasepassword"
   }
 }
}
```

**注意:** 由于服务与密钥路径相匹配，服务可以访问密钥。更多有关路径的信息，请参看[空间](/1.10/security/ent/#spaces).

3. 将文件保存为一个能说明意义的名字，例如`myservice.json`.

4. 通过DC/OS命令行部署豆荚，如下所示.
    
```bash
dcos marathon pod add mypod.json
```

5. 打开DC/OS图形用户界面.

6. 点击您的服务组名, 例如 **developer**.

7. 点击您的豆荚名.

8. 点击打开 **Configuration** 标签.

9. 在**环境变量** 区域内找到您的 `DCOS_SECRETS_DIRECTIVE`.
