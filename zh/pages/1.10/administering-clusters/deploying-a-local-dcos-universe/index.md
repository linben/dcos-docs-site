---
layout: layout.pug
navigationTitle: 部署本地Universe
title: 部署本地Universe
menuWeight: 1000
excerpt: ""
enterprise: false
preview: true
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

您可以在数据中心上安装和运行 DC/OS 服务, 而无需使用本地 [ 宇宙 ](https://github.com/mesosphere/universe) 进行 internet 访问。 您可以部署包括所有认证的软件包 (最简单), 或包括选定的包 (高级) 的本地宇宙。

**基础要求**

* 已安装[DC/OS CLI](/1.10/cli/install/)。

* 已登录到 DC/OS CLI。在 DC/OS 企业中, 您必须以 ` dcos: 超级用户 ` 权限登录。

** 注意: **由于宇宙压缩是超过 2 gb 的大小, 它可能需要一些时间下载到您的本地驱动器, 并上传到每个主机。

# <a name="certified"></a>部署包含认证的宇宙包的本地宇宙

1. 在终端提示符下, 使用以下命令将本地宇宙及其服务定义下载到您的本地驱动器上。
    
    ```bash
curl -v https://downloads.mesosphere.com/universe/public/local-universe.tar.gz -o local-universe.tar.gz
curl -v https://raw.githubusercontent.com/mesosphere/universe/version-3.x/docker/local-universe/dcos-local-universe-http.service -o dcos-local-universe-http.service
curl -v https://raw.githubusercontent.com/mesosphere/universe/version-3.x/docker/local-universe/dcos-local-universe-registry.service -o dcos-local-universe-registry.service
```

2. 使用 [ 安全副本 ](https://linux.die.net/man/1/scp) 将宇宙和注册表文件传输到主节点, 在发出以下命令之前, 将 ` < 主-ip > ` 替换为主机的公共 ip 地址。
    
    **提示：**您可以在DC / OS Web界面的左上角找到主设备的公共IP地址。
    
    ```bash
scp local-universe.tar.gz core@<master-IP>:~
scp dcos-local-universe-http.service core@<master-IP>:~
scp dcos-local-universe-registry.service core@<master-IP>:~
```

3. 使用以下命令将[ SSH ](/1.10/administering-clusters/sshcluster/)添加到主服务器中。 用前面命令中使用的IP地址替换`<master-IP> `。
    
    ```bash
ssh -A core@<master-IP>
```

4. 确认文件已成功复制。
    
        ls
        

5. 您应该看到列出下列文件。
    
        dcos-local-universe-http.service  dcos-local-universe-registry.service  local-universe.tar.gz
        

6. 将注册表文件移动到` /etc/systemd/system/ `目录中。
    
        sudo mv dcos-local-universe-registry.service /etc/systemd/system/
        sudo mv dcos-local-universe-http.service /etc/systemd/system/
        

7. 确认文件已成功复制到`/etc/systemd/system/`中。
    
    ```bash
ls -la /etc/systemd/system/dcos-local-universe-*
```

8. 将Universe加载到本地Docker实例中。
    
    ```bash
docker load < local-universe.tar.gz
```

**提示**：这可能需要一些时间才能完成。

9. 重新启动systemd守护进程。
    
    ```bash
sudo systemctl daemon-reload
```

10. 启用并启动` dcos-local-universe-http `和` dcos-local-universe-registry `服务。
    
    ```bash
sudo systemctl enable dcos-local-universe-http
sudo systemctl enable dcos-local-universe-registry
sudo systemctl start dcos-local-universe-http
sudo systemctl start dcos-local-universe-registry
```

11. 使用以下命令确认服务已经启动并正在运行。
    
    ```bash
sudo systemctl status dcos-local-universe-http
sudo systemctl status dcos-local-universe-registry
```

12. 如果您只有一个主人，请跳到第25步。如果您有多个主人，请继续下一步。

13. 使用以下命令发现所有主设备的私有IP地址。 从列表中确定您现在被SSH连接的主机的私有IP地址。
    
    **提示：**在提示符中，它将匹配` core @ ip - `后面显示的路径，连字符将变成句点。
    
        host master.mesos
        

14. 使用[安全副本](https://linux.die.net/man/1/scp)将Universe和注册表文件转移到其他主人之一。 将`< master-IP>`替换为另一个主设备的IP地址。
    
    ```bash
scp local-universe.tar.gz core@<master-IP>:~
scp /etc/systemd/system/dcos-local-universe-registry.service core@<master-IP>:~
scp /etc/systemd/system/dcos-local-universe-http.service core@<master-IP>:~
```

15. 将[ SSH ](/1.10/administering-clusters/sshcluster/)添加到您刚刚将这些文件复制到的主文件夹中。
    
    ```bash
ssh -A core@<master_IP>
```

16. 确认文件已成功复制。
    
        ls
        

17. 您应该看到列出下列文件。
    
        dcos-local-universe-http.service  dcos-local-universe-registry.service  local-universe.tar.gz
        

18. 将注册表文件移动到` /etc/systemd/system/ `目录中。
    
        sudo mv dcos-local-universe-registry.service /etc/systemd/system/
        sudo mv dcos-local-universe-http.service /etc/systemd/system/
        

19. 确认文件已成功复制到`/etc/systemd/system/`中。
    
    ```bash
ls -la /etc/systemd/system/dcos-local-universe-*
```

20. 将Universe加载到本地Docker实例中。
    
        docker load < local-universe.tar.gz
        
    
    **提示**：这可能需要一些时间才能完成。

21. 重新启动Docker守护进程。
    
    ```bash
sudo systemctl daemon-reload
```

22. 启动` dcos-local-universe-http `和` dcos-local-universe-registry `服务。
    
    ```bash
sudo systemctl start dcos-local-universe-http
sudo systemctl start dcos-local-universe-registry
```

23. 使用以下命令确认服务已经启动并正在运行。
    
    ```bash
sudo systemctl status dcos-local-universe-http
sudo systemctl status dcos-local-universe-registry
```

24. 重复第14步到第23步，直到完成了所有主人的这一步骤。 然后继续下一步。

25. 输入` exit `关闭SSH会话或打开新的终端提示符。
    
    **提示**：如果您有多个主设备，则可能必须退出多个SSH会话。

26. (可选) 使用以下命令从集群中删除对默认Universe的引用。 如果您想要保留默认的Universe，只需将本地Universe添加为其他存储库，请跳至下一步。
    
    ```bash
dcos package repo remove Universe
```

**提示**：您也可以在DC / OS Web界面中从**设置**> **程序包存储库**中删除对默认Universe存储库的引用。

27. 使用以下命令添加对添加到每个主控的本地Universe的引用。
    
    ```bash
dcos package repo add local-universe http://master.mesos:8082/repo
```

28. [SSH到您的代理节点之一。](/1.10/administering-clusters/sshcluster/)
    
    ```bash
dcos node ssh --master-proxy --mesos-id=<mesos-id>
```

29. 使用以下命令在本地下载DC / OS证书的副本并将其设置为可信。
    
    ```bash
sudo mkdir -p /etc/docker/certs.d/master.mesos:5000
sudo curl -o /etc/docker/certs.d/master.mesos:5000/ca.crt http://master.mesos:8082/certs/domain.crt
sudo systemctl restart docker
```

30. 配置Apache Mesos fetcher以信任下载的Docker证书。
    
    1. 复制证书：
        sudo cp /etc/docker/certs.d/master.mesos:5000/ca.crt /var/lib/dcos/pki/tls/certs/docker-registry-ca.crt
        
    
    1. 生成一个哈希：
        cd /var/lib/dcos/pki/tls/certs/
        openssl x509 -hash -noout -in docker-registry-ca.crt
        
    
    1. 创建一个软链接：
        sudo ln -s /var/lib/dcos/pki/tls/certs/docker-registry-ca.crt /var/lib/dcos/pki/tls/certs/<hash_number>.0
        
    
    **注意：**您需要在公共代理上创建` /pki/tls/certs`目录。

31. 输入` exit `关闭SSH会话或打开新的终端提示符。 在每个代理节点上重复步骤28-30。

32. 要验证您的成功，请登录到DC / OS Web界面，然后单击**目录**选项卡。 你应该看到一个认证软件包列表。 安装其中一个软件包。

### 常问问题

* **我无法安装CLI子命令**
    
    软件包托管在` master.mesos:8082 `中。 如果您无法通过DC / OS CLI安装解决或连接到` master.mesos:8082 `，则无法安装CLI子命令。 如果您可以连接到主站上的端口8082，请将其中一个主站的IP添加到`/etc/hosts`中。

* **images被打破**
    
    所有Universe组件都托管在群集中，包括图像。 组件由` master.mesos:8082 `提供。 如果连接到该IP，则可以将其添加到` /etc/hosts `中，并使图像正常工作。

* **我没有看到我正在寻找的包**
    
    默认情况下，只有认证包被捆绑在一起。 如果您想获得其他内容，请使用下一节中的说明。

# <a name="build"></a>部署包含选定软件包的本地Universe

**先决条件：** [ Git ](https://git-scm.com/)。 在 Unix/Linux 上, 请参阅以下 <a href="https://git-scm.com/book/en/v2/Getting-Started-Installing-Git" target="_blank"> 安装说明 </a>。

要部署包含您自己的软件包集的本地宇宙, 您必须构建一个自定义的本地宇宙泊坞窗图像。

1. 克隆Universe存储库：
    
    ```bash
git clone https://github.com/mesosphere/universe.git --branch version-3.x
```

2. 构建` universe-base `图片：
    
    ```bash
cd universe/docker/local-universe/
sudo make base
```

3. 构建`mesosphere/universe` Docker镜像并将其压缩到`local-universe.tar.gz` 文件。 使用 ` DCOS_PACKAGE_INCLUDE ` 变量指定一个以逗号分隔的包名称和版本的列表。 要最小化容器大小和下载时间, 您只能选择所需的内容。 如果不使用 ` DCOS_PACKAGE_INCLUDE ` 变量, 则包括所有认证的宇宙软件包。 要查看已验证的软件包, 请单击 "DC/OS web 界面" 中的 ** 目录 ** 选项卡。
    
    ```bash
sudo make DCOS_VERSION=1.10 DCOS_PACKAGE_INCLUDE="cassandra:1.0.25-3.0.10,marathon:1.4.2" local-universe
```

4. 执行除[部署包含Certified Universe程序包的本地Universe](#certified)之外的所有步骤（第27步除外）。将步骤27中的命令替换为以下内容。
    
    ```bash
dcos package repo add local-universe http://master.mesos:8082/repo
```