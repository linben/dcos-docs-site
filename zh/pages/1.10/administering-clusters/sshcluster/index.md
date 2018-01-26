---
layout: layout.pug
navigationTitle: SSHing 到节点
title: SSHing 到节点
menuWeight: 0
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

这些说明解释了如何从外部网络设置到您的 DC/OS 群集的 SSH 连接。 如果您与群集处于同一网络或使用 VPN 连接, 则可以改用 ` dcos 节点 ssh ` 命令。 有关更多信息, 请参见 cli 引用的 [ dcos 节点节 ](/1.10/cli/command-reference/)。

* [SSH 到您的 DC/OS 群集在 Unix/Linux (macOS, Ubuntu 等)](#unix)
* [SSH 到您的 DC/OS 群集在窗口](#windows)

**需求：**

* 一个未加密的 ssh 密钥, 可用于通过 ssh 对群集节点进行身份验证。不支持加密的 SSH 密钥。

### <a name="unix"></a>SSH 到您的 DC/OS 群集在 Unix/Linux (macOS, Ubuntu 等)

1. 使用 ` chmod ` 命令将 `. pem ` 文件上的权限更改为所有者读/写。
    
    ** 重要: **您的 `. pem ` 文件必须位于 `/. ssh ` 目录中。
    
    ```bash
chmod 600 <private-key>.pem
```

2. SSH 进入集群。
    
    1. 在终端中, 将新配置添加到 `. pem ` 文件中, 其中 ` < 私钥 > ` 是您的 `. pem ` 文件。
        
        ```bash
ssh-add ~/.ssh/<private-key>.pem
Identity added: /Users/<yourdir>/.ssh/<private-key>.pem (/Users/<yourdir>/.ssh/<private-key>.pem)
```

* **To SSH to a master node:**
    
    1. From the DC/OS CLI, enter the following command:
        
        ```bash
dcos node ssh --master-proxy --leader
```
    
    **Tip:** The default user is `core` for CoreOS. If you are using CentOS, enter:
    
    ```bash
dcos node ssh --master-proxy --leader --user=centos
```

* **To SSH to an agent node:**
    
    1. From the DC/OS CLI, enter the following command, where `<mesos-id>` is your agent ID.
        
        ```bash
dcos node ssh --master-proxy --mesos-id=<mesos-id>
```
    
    **Tip:** To find the agent ID, select the **Nodes** tab in the DC/OS [web interface](/1.10/gui/) and click **Details**.
    
    ![Web interface node ID](/1.10/img/ssh-node-id.png)

### <a name="windows"></a>SSH 到您的 DC/OS 群集在窗口

**需求：**

* PuTTY SSH client or equivalent (These instructions assume you are using PuTTY, but almost any SSH client will work.)
* PuTTYgen RSA 和 DSA 密钥生成实用程序
* 选美 SSH 认证代理

To install these programs, download the Windows installer <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">from the official PuTTY download page.</a>

1. 使用 PuTTYgen 将 `. pem ` 文件类型转换为 `. ppk `:
    
    1. 打开 PuTTYgen, 选择 ** 文件 > 加载私钥 **, 然后选择您的 `. pem ` 文件。
    
    2. 选择 ** SSH-2 RSA ** 作为密钥类型, 单击 ** 保存私钥 **, 然后选择名称和位置以保存新的. ppk 密钥。
        
        ![Windows](/1.10/img/windowsputtykey.png)
    
    3. 关闭 PuTTYgen。

2. SSH 进入集群。
    
    * **到主节点的 SSH:**
        
        1. 在 DC/OS web 界面中, 复制主节点的 IP 地址。它将是您用来连接到 GUI 的 IP 地址。
        
        2. Open PuTTY and enter the master node IP address in the **Host Name (or IP address)** field.
            
            ![Putty Configuration](/1.10/img/windowsputtybasic.png)
        
        3. In the **Category** pane on the left side of the PuTTY window, choose **Connection > SSH > Auth**, click **Browse**, locate and select your `.ppk` file, then click **Open**.
            
            ![Putty SSH Options](/1.10/img/windowsputtysshopt.png)
        
        4. Login as user "core" if you're running CoreOS. The default user on CentOS is "centos".
            
            ![Windows Login](/1.10/img/windowscore.png)
    
    * **到主节点的 SSH**
        
        **Prerequisite:** You must be logged out of your master node.
        
        1. Enable agent forwarding in PuTTY.
            
            **Caution:** SSH agent forwarding has security implications. Only add servers that you trust and that you intend to use with agent forwarding. For more information on agent forwarding, see <a href="https://developer.github.com/guides/using-ssh-agent-forwarding/" target="_blank">Using SSH agent forwarding.</a>
            
            1. Open PuTTY. In the **Category** pane on the left side of the PuTTY window, choose **Connection > SSH > Auth** and check the **Allow agent forwarding** box.
            
            2. Click the **Browse** button and locate the `.ppk` file that you created previously using PuTTYgen.
                
                ![Windows Forwarding](/1.10/img/windowsforwarding.png)
        
        2. 将 `. ppk ` 文件添加到选美中。
            
            1. 公开选美 如果没有出现 "选美" 窗口, 请在 "时钟" 旁边的屏幕右下区域的通知区域中查找选美图标, 然后双击它以打开 "选美" 的主窗口。
            
            2. 选择“运行”****按钮。
            
            3. 找到使用 PuTTYgen 创建的 `. ppk ` 文件, 然后单击 ** 打开 ** 将您的密钥添加到选美中。
                
                ![橱窗庆典](/1.10/img/windowspageant.png)
            
            4. 单击 ** 关闭 ** 按钮关闭 "选美" 窗口。
        
        3. SSH 进入主节点。
            
            1. 在 DC/OS web 界面中, 复制主节点的 IP 地址。IP 地址显示在群集名称的下面。
            
            2. 在 "腻子" 窗口左侧的 ** 类别 ** 窗格中, 选择 ** 会话 **, 然后在 ** 主机名 (或 IP 地址) ** 字段中输入主节点 IP 地址。
            
            3. 登录为用户 "核心", 如果你正在运行 CoreOS。CentOS 上的默认用户是 "CentOS"。
                
                ![窗口登录](/1.10/img/windowscore.png)
        
        4. 从主节点, SSH 进入代理节点。
            
            1. 从 Mesos web 界面中复制代理节点主机名。 您可以在 ** 框架 ** (` < 主节点-地址 >/mesos/#/框架 `) 或 ** 从属 ** 页 (` < 主节点-地址 >/mesos/#/奴隶 `) 中找到主机名。
            
            2. SSH 进入代理节点, 作为用户 ` 核心 `, 并指定代理节点主机名:
                
                    ssh core@ < 代理-节点-主机名 >