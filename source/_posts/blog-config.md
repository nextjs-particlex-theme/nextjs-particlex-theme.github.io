---
title: 博客配置
date: 2024/08/01 10:40:13
tags: 配置
seo:
  description: particlex 的博客文章配置项文档。
  keyword: 
    - 'particlex config'
    - 'particlex 配置'
---


> 说明: 类型中有 `undefined` 表示你可以不提供该参数。

# 博客配置

## 配置文件

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

theme_config: <undefined | ThemeConfig>
	# 首页背景图片，配置多个每次都会随机展示
	background: <string[] | undefined>
	# 首页显示的文章数量，默认为 5
	indexPageSize: <number | undeinfed>
	# 头像
	avatar: <string | undefined>
```

# 环境变量

一切以该文件注释为准：[environment.d.ts](https://github.com/nextjs-particlex-theme/particlex/blob/master/environment.d.ts).

## BLOG_PATH

**类型:** `string`

**说明:** 博客的路径，必须提供，虽然可以使用相对路径，但还是**强烈建议使用绝对路径**！

## NEXT_PUBLIC_CND_PUBLIC_PATH_BASE_URL

**类型:** `string` | `undefined`

**默认值:** 无默认值

**说明:** CDN 路径前缀，不要以 `/` 结尾，否则路径拼接会出问题。

> 因为环境变量是 next.js 处理的，所以没有很好的办法来对 `/` 结尾的 URL 进行替换...

## DATASOURCE_CACHE_ENABLE

**类型:** `boolean` | `undefined`

**默认值:** `true`

**说明:** 开启数据源缓存以加速构建，推荐开启 (使用默认值即可)。

## YAML_INDENT_SPACE_COUNT

**类型:** `number` | `undefined`

**默认值:** `2`

**说明:** 处理 [文章元数据](#文章元数据)中 yaml 配置的 TAB 缩进，这里会将 TAB 替换为相应数量的空格，以便于继续 YAML 解析。

> 虽然提供了这个配置，但还是不要用 TAB 键来敲 YAML 的缩进！！！除非你是在专门的 IDE 中，例如
> JetBrains 系列的 IDE 会在编辑 YAML 时自动将 TAB 转为 空格。
> **但是在 VSCode 中，这个转换是不存在的，因为你是在 md 文件中编辑的 yaml**，所以需要格外注意，同理其它编辑器也需要一并注意。


## NEXT_PUBLIC_COMMENT_SCRIPT_INJECT

**类型:** `string` | `undefined`

**默认值:** 无默认值

**说明:** 为第三方评论组件注入脚本。

详见 [配置评论组件](/tips#配置评论组件)

## NEXT_PUBLIC_COMMENT_CONTAINER_IDENTIFIER

**类型:** `string` | `undefined`

**默认值:** 无默认值

**说明:** 为第三方评论组件设置组件容器。

详见 [配置评论组件](/tips#配置评论组件)

## BLOG_HOME_POST_DIRECTORY

**类型:** `string`

**默认值:** `source/_posts`

**说明:** 首页文章的存放目录，相对于 [BLOG_PATH](#blog_path) 的路径。详见 [从任意 Markdown 博客迁移](/quick-start#从任意 Markdown 博客迁移)。

## BLOG_RESOURCE_DIRECTORY

**类型:** `string`

**默认值:** `source/images`

**说明:** 资源文件的存放目录，相对于 [BLOG_PATH](#blog_path) 的路径。详见 [从任意 Markdown 博客迁移](/quick-start#从任意 Markdown 博客迁移)。

## BLOG_POST_DIRECTORY

**类型:** `string`

**默认值:** `source`

**说明:** 所有博客文件的存放路径，相对于 [BLOG_PATH](#blog_path) 的路径。详见 [从任意 Markdown 博客迁移](/quick-start#从任意 Markdown 博客迁移)。

# 文章元数据


在文章 markdown 顶部使用如下格式以提供元数据：

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