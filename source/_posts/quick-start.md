---
title: 快速开始
date: 2024/08/17 22:57:15
categories: 教程 
seo:
	description: particlex 主题快速开始。
---


# 开始构建

本项目实际上就是一个普通的 [Next.js](https://nextjs.org/) 项目，通过在[环境变量](/blog-config#环境变量)中指向博客的目录，在构建时进行读取，最后借助于 SSG 模式生成博客的静态文件。

## 创建配置文件

> [!NOTE]
> 此步骤可选，不提供也可以，但是可能会有部分功能上的影响。


本博客框架最大的特点就是无侵入，你只需要拉取代码后，安装依赖并配置好相关环境变量后就可直接打包，但仍然需要你创建一个配置文件。

在你的博客根目录创建一个 `_config.yaml`，然后提供下面的配置：

```yaml
title: 博客标题
subtitle: 子标题
description: 描述
```


## 配置环境变量

### 从 Hexo 迁移

如果你打算从 Hexo 博客迁移，只需要提供一个环境变量即可无侵入迁移。


- 如果你是 Linux 系统，你也可以通过 `export` 关键字来临时暴露这个环境变量：

  ```shell
  export BLOG_PATH=/path/to/your/hexo
  ```

- 如果你是 windows，可以在**本项目**的根目录中创建一个 `.env.local` 文件：

  ```env
  # .env.local
  BLOG_PATH=/path/to/your/hexo
  ```


### 从其它任意 Markdown 博客迁移

支持从任意 Markdown 类型的博客迁移，只要你的博客文件是由 Markdown 文件编写的，就可以进行迁移。

迁移时，除了基础的 `BLOG_PATH` 需要提供外(详见上面的 [从 Hexo 迁移](#从-hexo-迁移))，还需要额外提供下面三个环境变量：

- `BLOG_HOME_POST_DIRECTORY`: 首页文章文件夹，这些文件将会在首页和归档中展示。
- `BLOG_RESOURCE_DIRECTORY`: 资源文件夹，访问前缀固定为 `images`。
- `BLOG_POST_DIRECTORY`：存放所有其它文章的文件夹，将会为这些页面构建 web 静态页面，但不会在首页会和归档页面中显示。

`BLOG_HOME_POST_DIRECTORY` 可以是 `BLOG_POST_DIRECTORY` 的子目录，**但是反过来不行**！除此之外，这两个目录也可以单独分开指定。

---

以 hexo 博客为例，上面三个参数应该分别配置为：

- `BLOG_HOME_POST_DIRECTORY`: `source/_posts`
- `BLOG_RESOURCE_DIRECTORY`: `source/images`
- `BLOG_POST_DIRECTORY`：`source`

在 hexo 中，`source` 目录中存放了所有的 Markdown 文档，`source/_post` 存放了首页文章的 Markdown 文档。

---

再例如，以下面的博客目录结构为例：

```text
blog-root
├── ROOT
│   ├── java_guide.md
│   └── javascript_guide.md
├── javascript
│   └── javascript_advance.md
├── img
│   └── coffee.png
├── hello_world.md
├── java_advance.md
└── _config.yaml
```

其中 ROOT 目录中的 md 文件为 `首页文章`，其它文件均为普通的 `文章`(嵌套页面)。

相应的配置为：

- `BLOG_HOME_POST_DIRECTORY`: `ROOT`
- `BLOG_RESOURCE_DIRECTORY`: `img`
- `BLOG_POST_DIRECTORY`：`./`
- `BLOG_PATH`: `/xxx/xxx/blog-root`

注意 `BLOG_POST_DIRECTORY` 不能为空字符串。

> [!IMPORTANT]
> 提示：这里的静态资源目录虽然为 `img`，但实际访问前缀会被替换为 `images`，例如这里的 `img/coffee.png`，访问路径为 `images/coffee.png`. 这是 next.js 的限制，目前没有很优雅的方法解决。

## 开始构建

### 手动构建

在完成上面的配置后，直接运行下面的命令就可以开始构建了：

```shell
npm run install
npm run build
```

运行完成后，会在 `out` 目录生成所有的静态文件。

### 自动化构建

参考：[Github Pages 部署](/github-pages)。


# 常见问题

## 分支选择

应该优先考虑以 `v<version>-stabled` 分支作为基础来对博客进行构建，例如 `v1-stabled`，我们会持续对这个分支进行稳固更新。

这样你就可以不用考虑频繁的小版本的更新是否需要应用到自己的博客上，你只需要关注当有新的大版本发布时是否需要更新就可以了。

## 添加图片

> **强烈建议使用 CDN 保存你的图片，而不是直接和博客静态文件一起分发。**

由于 next.js 限制 (其实有解，但是不太优雅)，所有的图片必须放到 `images` 目录中，可以手动配置，但是访问的路径前缀将始终为 `images`。

例如在 hexo 中，图片必须全部放到 `source/images` 中，**放在其它任何地方的图片都会被忽略**。

---

所有的图片都使用了原生懒加载 `loading="_lazy"`。

## 设置网站图标

对于网站图标 `favicon.ico` 可以直接放在根目录中。

例如在 hexo 中需要放在 `source/favicon.ico` 中。

## 提示: 小心使用 > 和 <

当你需要使用 `>` 和 `<` 时，请万分小心，如果这两个符号没有被 \`、\`\`\`、或转移符号包括或修饰的话，在构建时将会报错：

``````markdown
<hello>         # illegal, will cause build failed.
&lt;hello&gt;   # ok
\<hello\>       # ok
`<hello>`       # ok

```             # ok
<hello>
```
``````

## 配置评论组件

配置评论组件需要设置两个[环境变量](/blog-config#环境变量)：

- `NEXT_PUBLIC_COMMENT_SCRIPT_INJECT`
- `NEXT_PUBLIC_COMMENT_CONTAINER_IDENTIFIER`

下面以 [giscus](https://giscus.app/zh-CN) 为例进行演示。

进入 [giscus](https://giscus.app/zh-CN) 配置网站：https://giscus.app/zh-CN 。按照要求配置完成后获取到一段 script 脚本：

```html
<script src="https://giscus.app/client.js"
    data-repo="nextjs-particlex-theme/nextjs-particlex-theme.github.io"
    data-repo-id="R_kgDOMcWxsw"
    data-category="Announcements"
    data-category-id="DIC_kwDOMcWxs84Chidp"
    data-mapping="pathname"
    data-strict="0"
    data-reactions-enabled="1"
    data-emit-metadata="0"
    data-input-position="top"
    data-theme="preferred_color_scheme"
    data-lang="zh-CN"
    data-loading="lazy"
    crossorigin="anonymous"
    async>
```

同时 `giscus` 要求网页中必须要拥有一个类名为 `giscus` 元素来作为评论组件的容器，所以最终的环境变量的值为：

- `NEXT_PUBLIC_COMMENT_SCRIPT_INJECT`: `<上面那串脚本>`
- `NEXT_PUBLIC_COMMENT_CONTAINER_IDENTIFIER`: `.giscus`


以 Github Pages 为例，在构建阶段，配置文件就要这么写：

```yaml
- name: Build
  env:
    NEXT_PUBLIC_COMMENT_SCRIPT_INJECT: |
      <script src="https://giscus.app/client.js"
              data-repo="nextjs-particlex-theme/nextjs-particlex-theme.github.io"
              data-repo-id="R_kgDOMcWxsw"
              data-category="Announcements"
              data-category-id="DIC_kwDOMcWxs84Chidp"
              data-mapping="pathname"
              data-strict="0"
              data-reactions-enabled="1"
              data-emit-metadata="0"
              data-input-position="top"
              data-theme="preferred_color_scheme"
              data-lang="zh-CN"
              data-loading="lazy"
              crossorigin="anonymous"
              async>
      </script>
    NEXT_PUBLIC_COMMENT_CONTAINER_IDENTIFIER: '.giscus'
```

## 配置 CDN

如果需要配置 CDN，你需要缓存项目中 [public](/public) 目录中所有内容到你的 CDN 中，并最终在构建阶段提供环境变量。例如在 Github Actions中：

```
- name: Build
  run: 'snip...'
  env: 
    NEXT_PUBLIC_CND_PUBLIC_PATH_BASE_URL: 'https://your.cdn/prefix'
```
> [!TIP]
> public 目录在一个 stable 分支中不会发生变动，当你使用新的分支时建议参考迁移指南来判断是否需要更新你的 CDN 数据。