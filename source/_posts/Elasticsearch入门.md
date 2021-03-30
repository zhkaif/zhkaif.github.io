---
title: Elasticsearch入门
date: 2021-03-23 11:38:04
tags:
  - Elasticsearch
  - 笔记
categories: [DB, Elasticsearch]
---

## Elasticsearch 简介

Elasticsearch 是一款分布式搜索和分析引擎，为所有类型的数据提供快速近乎于实时的搜索和分析。无论是结构化文本还是非结构化文本，数字数据或地理空间数据，Elasticsearch 都能快速有效的对其进行存储和索引。

### 应用场景

- 应用或者网站的搜索框
- 存储和分析日志、指标和安全事件数据
- 机器学习用来实时自动模拟数据行为
- 作为存储引擎，实现业务工作流的自动化
- 作为地理信息系统（GIS）来管理、整合和分析空间信息
- 作为生物信息学研究工具存储和处理遗传数据

### 数据存储：文档和索引

Elasticsearch 不会将信息存储为列数据的行，而是存储已序列化为 JSON 文档的复杂数据结构。索引可以看做是文档的优化集合，每个文档都是字段的集合，这些字段包含数据的键值对。

Elasticsearch 为每个字段中的所有数据建立索引，并且每个索引字段都具有专门的优化数据结构。例如，文本字段存储在倒排索引中，数字字典和地理字段存储在 BKD 树中。

## Elasticsearch 安装

### 下载

下载链接 <https://www.elastic.co/cn/downloads/elasticsearch>

### 安装使用

直接将下载的压缩包解压，启动`bin`目录下面的`elasticsearch.bat`

浏览器打开<http://localhost:9200/>查看 Elasticsearch 服务信息

![avatar](https://img.imgdb.cn/item/6061419f8322e6675c770dcf.jpg)

### 配置远程访问

打开`config`目录下的`elasticsearch.yml`配置文件

`修改network.host: 你的IP地址`

放开注释`#cluster.initial_master_nodes: ["node-1", "node-2"]`
节点根据情况进行增删
