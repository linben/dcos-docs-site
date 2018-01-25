---
layout: layout.pug
navigationTitle: 授予对作业的访问权限
title: 授予对作业的访问权限
menuWeight: 200
excerpt: ""
enterprise: true
---
通过使用 DC/OS GUI、CLI 或 [ api ](/1.10/security/ent/iam-api/), 可以实现对作业的细粒度用户访问。 [节拍器权限](/1.10/security/ent/perms-reference/#marathon-metronome)允许您限制用户在每个作业或每个作业组的基础上访问作业。 本节将引导您完成此操作。

**基础要求**

- 必须安装 [ DC/OS cli ](/1.10/cli/install/), 并以超级用户身份登录。
- 要为其分配权限的 [ 用户帐户 ](/1.10/security/ent/users-groups/)。

# <a name="job-group-access-via-ui"></a>通过 DC/OS GUI

1. 以具有` superuser `权限的用户身份登录到DC / OS GUI。
    
    ![Login](/1.10/img/gui-installer-login-ee.gif)

2. 选择 ** Organization ** 并选择 ** Users ** 或 ** Groups **。

3. 选择要授予权限的用户或组的名称。
    
    ![Add permission cory](/1.10/img/services-tab-user.png)

4. 在 ** Permissions ** 选项卡中, 单击 **ADD PERMISSION**。

5. 单击 **INSERT PERMISSION STRING** 以切换对话框。
    
    ![Add permission](/1.10/img/services-tab-user3.png)

6. 将权限复制并粘贴到**权限字符串**字段中。 根据您的[安全模式](/1.10/security/ent/#security-modes)选择权限字符串。
    
    ### 禁用
    
    - **DC / OS作业访问：**
        
        指定您的作业组 (`<job-group>`)，作业名称(`<job-name>`)和动作(`<action>`)。 操作可以是 ` create `、` read `、` update `、` delete ` 或 ` full `。 To permit more than one operation, use a comma to separate them, for example: `dcos:service:metronome:metronome:jobs:<job-group>/<job-name> read,update`.
        
        ```bash
dcos:adminrouter:service:metronome full
dcos:service:metronome:metronome:jobs:<job-group>/<job-name> <action>
```

- **DC / OS服务任务和日志：**
    
    ```bash
dcos:adminrouter:ops:mesos full
dcos:adminrouter:ops:slave full
```

### 许可

- **DC / OS作业访问：**
    
    Specify your job group (`<job-group>`), job name (`<job-name>`), and action (`<action>`). Actions can be either `create`, `read`, `update`, `delete`, or `full`. To permit more than one operation, use a comma to separate them, for example: `dcos:service:metronome:metronome:jobs:<job-group>/<job-name> read,update`.
    
    ```bash
dcos:adminrouter:service:metronome full
dcos:service:metronome:metronome:jobs:<job-group>/<job-name> <action>
```

- **DC / OS服务任务和日志：**
    
    ```bash
dcos:adminrouter:ops:mesos full
dcos:adminrouter:ops:slave full
```

### 严格

- **DC / OS作业访问：**
    
    Specify your job group (`<job-group>`), job name (`<job-name>`), and action (`<action>`). Actions can be either `create`, `read`, `update`, `delete`, or `full`. To permit more than one operation, use a comma to separate them, for example: `dcos:service:metronome:metronome:jobs:<job-group>/<job-name> read,update`.
    
    ```bash
dcos:adminrouter:service:metronome full
dcos:service:metronome:metronome:jobs:<job-group>/<job-name> <action>
```

- **DC / OS服务任务和日志：**
    
    ```bash
dcos:adminrouter:ops:mesos full
dcos:adminrouter:ops:slave full
dcos:mesos:master:framework:role:* read
dcos:mesos:master:executor:app_id:/<job-group>/<job-name> read
dcos:mesos:master:task:app_id:/<job-group>/<job-name> read
dcos:mesos:agent:framework:role:* read
dcos:mesos:agent:executor:app_id:/<job-group>/<job-name> read
dcos:mesos:agent:task:app_id:/<job-group>/<job-name> read
dcos:mesos:agent:sandbox:app_id:/<job-group>/<job-name> read
```

7. Click **ADD PERMISSIONS** and then **Close**.

# <a name="job-group-access-via-cli"></a>通过CLI

**基础要求**

- 必须安装 [ DC/OS cli ](/1.10/cli/install/), 并以超级用户身份登录。

**提示:**

- To grant permissions to a group instead of a user, replace `users grant <user-name>` with `groups grant <group-name>`. 

### 禁用

This mode does not offer fine-grained control.

### 许可

- **DC / OS作业访问：**
    
    1. Grant the permission to job group (`<job-group>`) and job name (`<job-name>`).
        
        ```bash
dcos security org users grant <user-name> adminrouter:service:metronome full --description "Controls access to Metronome services"
dcos security org users grant <user-name> service:metronome:metronome:jobs:<job-group>/<job-name> full --description "Controls access to <job-group>/<job-name>"
```

- **DC / OS服务任务和日志：**
    
    1. Grant the permission to a user (`<user-name>`).
        
        ```bash
dcos security org users grant <user-name> adminrouter:ops:mesos full --description "Grants access to the Mesos master API/UI and task details"
dcos security org users grant <user-name> adminrouter:ops:slave full --description "Grants access to the Mesos agent API/UI and task details such as logs"
```

### 严格

- **DC / OS作业访问：**
    
    1. Grant the permission to job group (`<job-group>`) and job name (`<job-name>`).
        
        ```bash
dcos security org users grant <user-name> adminrouter:service:metronome full --description "Controls access to Metronome services"
dcos security org users grant <user-name> service:metronome:metronome:jobs:<job-group>/<job-name> full --description "Controls access to <job-group>/<job-name>"
```

- **DC / OS服务任务和日志：**
    
    1. 向用户 (`<user-name>`)和组(`<job-group>`)授予权限。
        
        ```bash
dcos security org users grant <user-name> adminrouter:ops:mesos full --description "Grants access to the Mesos master API/UI and task details"
dcos security org users grant <user-name> adminrouter:ops:slave full --description "Grants access to the Mesos agent API/UI and task details such as logs"
dcos security org users grant <user-name> mesos:master:framework:role:* read --description "Controls access to frameworks registered with the Mesos default role"
dcos security org users grant <user-name> mesos:master:executor:app_id:/<job-group>/<job-name> read --description "Controls access to executors running inside <job-group>/<job-name>"
dcos security org users grant <user-name> mesos:master:task:app_id:/<job-group>/<job-name> read --description "Controls access to tasks running inside <job-group>/<job-name>"
dcos security org users grant <user-name> mesos:agent:framework:role:* read --description "Controls access to information about frameworks registered under the Mesos default role"
dcos security org users grant <user-name> mesos:agent:executor:app_id:/<job-group>/<job-name> read --description "Controls access to executors running inside <job-group>/<job-name>"
dcos security org users grant <user-name> mesos:agent:task:app_id:/<job-group>/<job-name> read --description "Controls access to tasks running inside <job-group>/<job-name>"
dcos security org users grant <user-name> mesos:agent:sandbox:app_id:/<group-name>/ read --description "Controls access to the sandboxes of <job-group>/<job-name>"
```