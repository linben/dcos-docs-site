---
layout: layout.pug
navigationTitle: 外部暴露 Mesos 区域
title: 外部暴露 Mesos 区域
menuWeight: 300
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

有些情况下, 您可能希望在 dc/os 之外使用 dc/os 群集内部的 DNS 记录的服务。 但是, DC/OS 用于公开记录的 `. mesos ` 域名称不支持此项。 要启用此功能, 您可以将 BIND 服务器放在群集的前面。

每个 DC/OS 群集都有一个唯一的加密标识符。可以在 ** 概述 ** 下的 UI 中找到该标识符的 zbase32 编码版本。

在本示例中, 使用了加密群集 ID ` yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo `。

1. 在群集前面安装 BIND 服务器。

2. 为您的DC / OS主机创建一个类似于此的转发条目。
    
        zone "yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo.dcos.directory" {
                type forward;
                forward only;
                forwarders { 10.0.4.173; };  // <Master-IP-1;Master-IP-2;Master-IP-3>
        };
        

3. 使用分号分隔的主IP地址列表替换主IP(`< Master-IP> `)。

4. 用您自己的替换示例加密群集ID。

## 建立一个区域

现在，您可以创建您想要别名的区域。 您也可以跳过此步骤并[使用现有区域](#existing)。

1. 在` named.conf `文件中创建一个区域条目。 对于这个例子，使用` contoso.com `：
    
        zone "contoso.com" {
                type master;
                file "/etc/bind/db.contoso.com";
        };
        

2. 填充区域文件：
    
        $TTL    604800
        @       IN      SOA     localhost. root.localhost. (
                                      1         ; Serial
                                      1         ; Refresh
                                      1         ; Retry
                                      1         ; Expire
                                      1 )       ; Negative Cache TTL
        ;
        @       IN      NS      localhost.
        @       IN      DNAME   mesos.yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo.dcos.directory.
        

## <a name="existing"></a>使用现有的区域

- 要使用现有区域，请添加DNAME记录：
    
        @       IN      DNAME   mesos.yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo.dcos.directory.
        
    
    ` @ `别名为区域的顶层，例如` contoso.com `。

- 要为高级别域别名，请在DNAME记录中指定该值。 在这个例子中，` foo `别名` foo.contoso.com `：
    
        foo       IN      DNAME   mesos.yor6tqhiag39y6cjkdd4w9uzo45qhku6ra8hl7hpr6d9ukjaz3jo.dcos.directory.