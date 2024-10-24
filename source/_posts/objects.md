---
title: 配置对象
date: 2024/10/24 09:41:25
tags: 配置
seo:
  description: particlex 的博客文章配置项文档，包含了各个配置对象的详细信息。
  keyword: 
    - 'particlex config'
    - 'particlex 配置'
---


# DataSourceConfig

[Source Code](https://github.com/nextjs-particlex-theme/particlex/blob/master/src/api/datasource/types/definitions.ts)

| 名称            | 类型                              | 描述                             |
|---------------|---------------------------------|--------------------------------|
| title         | string                          | 博客名称                           |
| subtitle      | string \| null                 | 子标题                            |
| description   | string \| null                 | 描述                             |
| author        | string                        | 作者名称                           |
| homePage        | string \| null               | 作者首页链接                        |
| theme_config      | [ThemeConfig](#datasourceconfig) \| null | 主题配置            |
| metadata        | Map<string, string> | null     | 为每个页面添加自定义meta标签       |

## ThemeConfig

| 名称            | 类型                              | 描述                             |
|---------------|---------------------------------|--------------------------------|
| indexPageSize | number \| null                 | 首页分页大小，默认为5                    |
| background | string[] \| null                 | 首页背景图片，每次访问会随机展示                |
| avatar       | string \| null                 | 头像链接                         |
| favicon       | string \| null                 | 外部图标链接。不填默认使用根目录下的 favicon.ico |


# Envrionment


详见: [environment.d.ts](https://github.com/nextjs-particlex-theme/particlex/blob/master/environment.d.ts)

默认值：[.env](https://github.com/nextjs-particlex-theme/particlex/blob/master/.env)


# Metadata

| 名称            | 类型                           | 描述                             |
|---------------|---------------------------------|--------------------------------|
| title        | string \| null                   | 文章标题，不写时使用 `Untitled`                                           |
| date         | string \| null                   | 文件创建时间，使用 yyyy/MM/dd hh:mm:ss 的格式，例如: 2024/10/24 11:14:33    |
| categories   | string | string[] \| null        | 文章分类                         |
| tags         | string | string[] \| null        | 文章标签 |
| seo          | [SEO](#seo) \| null              | SEO 优化 |

## SEO


SEO 优化

| 名称            | 类型                           | 描述                             |
|---------------|---------------------------------|--------------------------------|
| title        | string \| null                   | **网页**标题                           |
| description        | string \| null                   | **网页**描述，推荐长度至少为 25      |
| keywords        | string[] \| null                   | **网页**关键词                      |
