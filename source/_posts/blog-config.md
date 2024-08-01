---
title: 博客配置
date: 2024/08/01 10:40:13
---

# 配置文件

> 由于做这个项目的目的是弃用 hexo，所以可以无缝从 hexo 切换过来。

在你的博客根目录中创建 `_config.yaml`.

可用配置：

```yaml
# 博客标题
title: <string>
# 子标题
subtitle: <string | undefined>
# 描述
description: <string | undefined>
# homePage
authorHome: <string | undefined>

theme_config:
	# 首页背景图片，配置多个每次都会随机展示
	background: <string[] | undefined>
	# 首页显示的文章数量，默认为 5
	indexPageSize: <number | undeinfed>
	# 头像
	avatar: <string | undefined>
```

# 环境变量

[environment.d.ts](https://github.com/nextjs-particlex-theme/particlex/blob/master/environment.d.ts)

# 博客元数据

在 markdown 顶部使用如下格式以提供元数据：

```text
---
<yaml content here>
---
```

在 `---` 之间的内容，就是 yaml 格式的元数据，与 hexo 兼容。

## 配置项

### title

类型：`string` | `undefined`

文章标题，可以为空，但是推荐提供。

### date

类型：`string ` | `undefined`

文章创建时间，任意可以被 js 使用 `new Date(<date>)` 解析的字符串，例如：`2023-09-04 16:16:53`，可以为空。

### tags

类型: `string[] ` | `string` |`undefined`

文章标签：

```yaml
# 只提供一个 tag
tags: java
---
# 提供多个 tag
tags:
	- java
	- bugfix
```

### categories

类型: `string[] ` | `string` |`undefined`

文章分类：

```yaml
# 只提供一个 tag
categories: java
---
# 提供多个 tag
categories:
	- java
	- springboot
---
# 这个写法似乎是 particlex 独有的，翻了半天官方文档没找到，不推荐使用，后续考虑删除...
# 为了保持统一，这个写法会忽略 path 的值
categories:
  data:
    - { name: "Rust", path: "/rust/" }
```

