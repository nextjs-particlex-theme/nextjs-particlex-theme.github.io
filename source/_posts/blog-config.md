---
title: 博客配置
date: 2024/08/01 10:40:13
tags: 配置
seo:
	description: particlex 的博客文章配置项文档。
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

**Overview:**

```yaml
title: async/await 异步编程
date: 2024-07-01 22:56:28
categories:
  - Rust
tags: Rust
seo:
  title: async/await 异步编程 | Rust
  description: 使用 async/await 进行异步编程。
  keywords: 
    - rust
    - async
    - await
```


### title

**类型**：`string` | `undefined`

文章标题，**虽然可以为空，但是强烈推荐提供**。

**示例:**

```yaml
title: Java Hello World
```


### date

**类型**：`string ` | `undefined`

文章创建时间，任意可以被 js 使用 `new Date(<date>)` 解析的字符串，例如：`2023-09-04 16:16:53`，可以为空。

**示例:**

```yaml
date: 2023-09-04 16:16:53
```

### tags

**类型**: `string[]` | `string` | `DeprecatedTags` |`undefined`

**详细类型定义**：

```yaml
tags: <string | string[] | undefined>
---
# 为了兼容 particlex 主题提供的，不推荐使用，实际实现会忽略 `path` 的值
tags:
  data:
    - { name: <string>, path: <string> }
```


**示例**:

```yaml
tags: java
---
tags: 
	- java
	- spring
---
tags:
  data:
    - { name: 'java', path: '/java/' }
```

### categories

**类型**: `string[] ` | `string` | `DeprecatedCategories` | `undefined`

**详细类型定义**：

```yaml
categories: <string | string[] | undefined>
---
# 为了兼容 particlex 主题提供的，不推荐使用，实际实现会忽略 `path` 的值
categories:
  data:
    - { name: <string>, path: <string> }
```

**示例**:

```yaml
categories: java
---
categories: 
	- java
	- spring
---
categories:
  data:
    - { name: 'java', path: '/java/' }
```

### seo

**类型**：`SEO | undefined`

**详细类型定义**：

```yaml
seo:
	title: <string | undefined>
	keywords: <string[] | undefined>
	description: string
```

- `title`: html 网页标题，而非文章的标题，如果不指定，则会使用 `<文章标题> | <博客title>`。
	
	参考资料：[影响标题链接的最佳实践](https://developers.google.cn/search/docs/appearance/title-link?hl=zh-cn#page-titles)。

- `keywords`: 关键字，如果不提供，默认使用 `tags` 内容(如果有)。

	参考资料: [关键词堆砌](https://developers.google.cn/search/docs/essentials/spam-policies?hl=zh-cn#keyword-stuffing)； [Google 不会将关键字元标记用于网页排名](https://developers.google.cn/search/blog/2009/09/google-does-not-use-keywords-meta-tag?hl=zh-cn)。

- `description`: 网页描述。

	参考资料：[使用高质量的描述](https://developers.google.cn/search/docs/appearance/snippet?hl=zh-cn#use-quality-descriptions)。

**示例:**

```yaml
seo:
	title: Java Hello World | Xxx's Blog
	keywords:
		- java
		- 'hello world'
	description: '使用 Java 编写 Hello World 代码。'
```