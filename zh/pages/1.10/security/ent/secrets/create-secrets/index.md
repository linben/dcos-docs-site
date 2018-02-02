---
layout: layout.pug
navigationTitle: Creating secrets
title: Creating secrets
menuWeight: 0
excerpt: ""
enterprise: true
---
您可以使用键值或文件的方式在DC/OS中创建密钥。这两种方法都将名称和键值添加到密钥商店中。如果你已经在本地文件中已经保存有密钥值，你可能会发现把密钥文件添加进去是很方便的。

请参阅[配置服务和豆荚以使用密钥](/1.10/security/ent/secrets/use-secrets/) 来获取有关如何在应用程序或豆荚中引用这些密钥的信息。

# 创建密钥

下面的部分将介绍如何使用图形界面，命令行界面和密钥API将密钥以键/值对和或以文件文件创建。

密钥应该包括路径，除非你想允许所有的服务来访问它。 有关密钥路径的更多信息，请参阅[空间](/1.10/security/ent/#spaces) 。

## 先决条件

### DC/OS图形界面

- `dcos:superuser` 权限.

### DC/OS 命令行或密钥 API

- See [Secret Store Permissions](/1.10/security/ent/perms-reference/#secrets) for the permissions needed to create secrets from the CLI or API. The permissions you configure must include the name of the secret the user is allowed to create. You must have one permission per secret. The secret name and permission name must match.
- 有关从命令行或API创建密钥所需的权限，请参阅[密钥商店权限](/1.10/security/ent/perms-reference/#secrets)。 您配置的权限必须包含用户允许创建的密钥所对应名称。 每个密钥必须有一个许可。 密钥名称和权限名称必须一致。

[安装DC/OS命令行](/1.10/cli/install/) 以及[安装DC/OS企业版命令行](/1.10/cli/enterprise-cli/#ent-cli-install)。

# <a name="ui"></a>使用图形界面创建键/值对密钥

1. 使用具有`dcos:superuser`权限的用户登录DC/OS图形界面.

2. 打开 **安全管理** 下的 **密钥管理** 标签.

3. 点击右上角 **+** 图标.
    
    ![New Secret](/1.10/img/new-secret.png)

4. 在 **ID** 框中, 填写您的密钥名称和路径，例如, **developer/my-secret**.
    
    ![Secret ID/Value Fields](/1.10/img/secret-id-value.png)

5. 在 **值** 框中输入或粘贴密钥.

6. 完成后，密钥如下图.
    
    ![Creating the Secret](/1.10/img/create-secret.png)

7. 点击 **创建**.

# <a name="api"></a>使用API创建键/值对密钥

以下过程介绍如何在 `developer` 路径下创建一个名为 `my-secret` 的密钥。

**注意:** 如果您的[安全模式](/1.10/installing/ent/custom/configuration/configuration-parameters/#security-enterprise) 是`permissive`或`strict`，在使用本节的curl命令前，需要先执行[获取DC/OS权威公钥包](/1.10/security/ent/tls-ssl/get-cert/) 中所列步骤. 如果您的[安全模式](/1.10/installing/ent/custom/configuration/configuration-parameters/#security-enterprise) 是`disabled`, 您需要从以下命令中删除`--cacert dcos-ca.crt`.

1. 使用`dcos auth login` 登录命令行.

2. 使用以下命令创建密钥
    
    ```bash
curl -X PUT --cacert dcos-ca.crt -H "Authorization: token=$(dcos config show core.dcos_acs_token)" -d '{"value":"very-secret"}' $(dcos config show core.dcos_url)/secrets/v1/secret/default/developer/my-secret -H 'Content-Type: application/json'
```

# <a name="cli"></a>使用DC/OS Enterprise CLI创建键/值对密钥

以下过程介绍如何使用DC/OS Enterprise CLI在 `developer` 路径下创建一个名为 `my-secret` 的密钥。

1. 使用`dcos auth login` 登录命令行.

2. 使用以下命令创建密钥
    
    ```bash
dcos security secrets create --value=top-secret developer/my-secret
```

# 使用DC/OS Enterprise CLI基于文件创建密钥

以下过程介绍如何使用DC/OS Enterprise CLI基于文件在 `developer` 路径下创建一个名为 `my-secret` 的密钥。

文件内容(以下为文件`my-secret.txt`)可以是任何文本内容

**注意:** 在DC/OS 1.10, 只能从DC/OS命令行上传文件作为密钥。密钥文件的最大约为1M，减去1K的密钥商店元数据. 

1. 使用`dcos auth login` 登录命令行.

2. 使用以下命令创建新密钥
    
    ```bash
dcos security secrets create -f my-secret.txt developer/my-secret
```

**注意:** 密钥文件的最大约为1M，减去1K的密钥商店元数据. 
