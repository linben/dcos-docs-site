---
layout: layout.pug
navigationTitle: 例子
title: 例子
menuWeight: 20
excerpt: ""
preview: true
enterprise: false
---
这些示例为作业提供了常见的使用方案。

**先决条件：**

- [ dc/os ](/1.10/installing/oss/) 和 [ dc/os cli 安装 ](/1.10/cli/install/)。

# <a name="create-job"></a>创建简单作业

此 JSON 创建了一个没有时间表的简单作业。

1. 创建具有以下内容的 JSON 文件。
    
    ```json
{
  "id": "my-job",
  "description": "A job that sleeps",
  "run": {
    "cmd": "sleep 1000",
    "cpus": 0.01,
    "mem": 32,
    "disk": 0
  }
}
```

2. 从 DC/OS CLI 添加作业。
    
    ```bash
dcos job add <my-job>.json
```

或者, 使用 API 添加作业。

```bash
curl -X POST -H "Content-Type: application/json" -H "Authorization: token=$(dcos config show core.dcos_acs_token)" $(dcos config show core.dcos_url)/service/metronome/v1/jobs -d@/Users/<your-username>/<myjob>.json
```

# <a name="create-job-schedule"></a>使用计划创建作业

**注意：**仅当您从DC / OS CLI或GUI添加作业时，此示例JSON才有效。 使用下面的[示例](#schedule-with-api)通过API创建预定作业。

1. 创建具有以下内容的 JSON 文件。
    
        {
            "id": "my-scheduled-job",
            "description": "A job that sleeps on a schedule",
            "run": {
                "cmd": "sleep 20000",
                "cpus": 0.01,
                "mem": 32,
                "disk": 0
            },
            "schedules": [
                {
                    "id": "sleep-nightly",
                    "enabled": true,
                    "cron": "20 0 * * *",
                    "concurrencyPolicy": "ALLOW"
                }
            ]
        }
        

2. 添加作业。
    
    ```bash
dcos job add <my-scheduled-job>.json
```

# <a name="schedule-with-api"></a>创建作业并使用 API 关联计划

1. 使用上面的[说明](#create-job)添加没有计划的作业。

2. 使用以下内容创建JSON文件。 这是你工作的时间表。
    
        {
            "concurrencyPolicy": "ALLOW",
            "cron": "20 0 * * *",
            "enabled": true,
            "id": "nightly",
            "nextRunAt": "2016-07-26T00:20:00.000+0000",
            "startingDeadlineSeconds": 900,
            "timezone": "UTC"
        }
        

3. 添加时间表并将其与作业相关联。 通过DC / OS CLI：
    
    ```bash
dcos job schedule add <job-id> <schedule-file>.json
```

通过API

```bash
curl -X POST -H "Content-Type: application/json" -H "Authorization: token=$(dcos config show core.dcos_acs_token)" $(dcos config show core.dcos_url)/service/metronome/v1/jobs/<job-id>/schedules -d@/Users/<your-username>/<schedule-file>.json
```

**注意：**您可以将计划与多个工作关联。

# 创建分区作业环境 (仅限企业)

在此示例中，使用DC / OS GUI创建分区作业环境。 这使您可以限制每个作业或每个作业组的用户访问权限。 作业是在名为` batch `的作业组中创建的，该作业是名为` dev `的作业组的子项。

    ├── dev
        ├── batch
            ├── job1
            ├── job2
    

然后将作业组分配给用户` Cory `和` Alice `来限制访问权限。

**基础要求**

- DC / OS使用[安全模式](/1.10/security/ent/#security-modes) `许可`或` strict `进行安装。
- 您必须以` superuser `身份登录。

1. 以具有` superuser `权限的用户身份登录到DC / OS GUI。
    
    ![Login](/1.10/img/gui-installer-login-ee.gif)

2. 创建分区作业。
    
    1. Select **Jobs** and click **CREATE A JOB**.
    2. In the **ID** field, type `dev.batch.job1`. 
    3. In the **Command** field, type `sleep 1000` (or another valid shell command) and click **CREATE A JOB**.
        
        ![Create job](/1.10/img/job-ex1.png)
        
        这将在DC / OS中的此目录结构中创建一个作业：**作业> dev>批处理>作业1 **。
    
    4. 点击右上角的** + **图标创建另一个作业。
        
        ![Create another job](/1.10/img/job-ex2.png)
    
    5. In the **ID** field, type `dev.batch.job2`.
    
    6. In the **Command** field, type `sleep 1000` (or another valid shell command) and click **CREATE A JOB**. You should have two jobs:
        
        ![create job](/1.10/img/job-ex3.png)

3. Run the jobs.
    
    1. Click **Jobs > dev > batch > job1** and click **Run Now**.
        
        ![Run job](/1.10/img/job-ex4.png)
    
    2. Click **Jobs > dev > batch > job2** and click **Run Now**.

4. 为作业分配权限。
    
    1. Select **Organization > Users** and create new users named `Cory` and `Alice`.
        
        ![Create user Cory](/1.10/img/service-group3.png)
    
    2. Select the user **Cory** grant access to `job1`.
    
    3. From the **Permissions** tab, click **ADD PERMISSION** and toggle the **INSERT PERMISSION STRING** button to manually enter the permissions.
        
        ![Add permissions cory](/1.10/img/job-ex5.png)
    
    4. 将权限复制并粘贴到**Permissions Strings**字段中。 Specify your job group (`dev/batch`), job name (`job1`), and action (`read`). Actions can be either `create`, `read`, `update`, `delete`, or `full`. To permit more than one operation, use a comma to separate them, for example: `dcos:service:metronome:metronome:jobs:/dev/batch/job1 read,update`.
        
        **重要：**您的[安全模式](/1.10/security/ent/#security-modes)必须是`许可`或`严格`。
        
        ```bash
dcos:adminrouter:service:metronome full
dcos:service:metronome:metronome:jobs:dev/batch/job1 read
dcos:adminrouter:ops:mesos full
dcos:adminrouter:ops:slave full
dcos:mesos:master:framework:role:* read
dcos:mesos:master:executor:app_id:/dev/batch/job1 read
dcos:mesos:master:task:app_id:/dev/batch/job1 read
dcos:mesos:agent:framework:role:* read
dcos:mesos:agent:executor:app_id:/dev/batch/job1 read
dcos:mesos:agent:task:app_id:/dev/batch/job1 read
dcos:mesos:agent:sandbox:app_id:/dev/batch/job1 read
```

5. Click **ADD PERMISSIONS** and then **Close**.
6. Repeat these steps for user **Alice**, replacing `job1` with `job2` in the permissions.

5. 注销并以新用户身份重新登录以验证权限。 用户现在应该在**作业**选项卡内具有对`dev/batch/job1`和`dev/batch/job2`的指定级别的访问权限。 例如，如果您以**爱丽丝**登录，则只能看到** jobs2 **：
    
    ![Alice job view](/1.10/img/job-ex6.png)