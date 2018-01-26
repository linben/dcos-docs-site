---
layout: layout.pug
navigationTitle: Feature Maturity
title: Feature Maturity
menuWeight: 10
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

The purpose of the feature maturity phases is to educate customers, partners, and Mesosphere field and support organizations about the maturity and quality of features.
功能成熟的目的是教育客户，合作伙伴和Mesosphere以及支持机构关于功能的成熟度和质量。

# <a name="criteria"></a>标准

## 完成度

**功能:** 功能完成

**界面:** 功能具有在生命周期的API，命令行界面，用户界面

**文档:** 功能具有相应文档. 例如, 管理指南, 开必指南, 发布说明.

## 质量

**功能测试:** 功能验证正确。

**系统测试:** 通过负载，浸泡，压力，峰值，故障和配置测试等来组合验证，以满足扩展性，可靠性和性能要求。

**Mesosphere内部测试:** 功能在Mesosphere内部生产环境中使用.

# <a name="phases"></a>阶段

## <a name="experimental"></a>实验

使用此功能需要自担风险。此功能可能被增加，修改，或删除.

### 完整性

* 功能还未完成
* API还未完成，如有更改，恕不另行通知
* 用户界面可能没有或不完整
* 文档可能没有或不完整

### 质量

* 有限的或没有做功能测试
* 有限的或没有做系统测试
* 有限的或没有做Mesosphere内部测试

## <a name="preview"></a>预览

功能有可能增加，修改，或是删除

### 完整性

* 功能已完成
* API还未完成，如有更改，恕不另行通知
* 用户界面可能没有或不完整
* 文档可能不完整

### 质量

* 健壮的功能测试
* 有限的或没有做系统测试
* 有限的或没有做Mesosphere内部测试

## <a name="stable"></a>稳定

### 完整性

* 功能已完成
* API已完成，修改会受到生命周期的影响
* 用户界面已完成
* 文档已完成

### 质量

* 健壮的功能测试
* 健壮的性能测试
* 健壮的故障测试
* 健壮的Mesosphere内部测试
