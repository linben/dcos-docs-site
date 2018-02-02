---
layout: layout.pug
navigationTitle: Secrets
title: Secrets
menuWeight: 60
excerpt: ""
enterprise: true
---
使用DC/OS企业版密钥商店来保护数据库密码，API密钥和私钥等敏感信息。将密钥存储在秘密路径中，仅允许的服务可以检索该值。

[授权Marathon服务](/1.10//security/ent/#spaces) 可以在部署时检索密钥并将其值存储在环境变量中。

此外，[密钥API](/1.10/security/ent/secrets/secrets-api/) 可以让您[加密](/1.10/security/ent/secrets/seal-store/)/[解密](/1.10/security/ent/secrets/unseal-store/) 密钥商店。

您还可以在[权限参考指南](/1.10/security/ent/perms-reference/#secrets) 部分找到有关秘密的信息。
