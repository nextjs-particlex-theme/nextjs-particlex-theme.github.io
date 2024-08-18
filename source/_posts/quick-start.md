---
title: 快速开始
date: 2024/08/17 22:57:15
categories: 教程 
seo:
	description: particlex 主题快速开始。
---

# 概念

在这里简单介绍一些概念，方便你后续进行理解和使用。

## 首页文章和文章

首页文章，是指在文章的根目录就可以看到的一些博客文章。点击上面导航栏中的 [Archives](/archives) 按钮，所有列出的文章就是首页文章，同时在该页面中的搜索功能，**可搜索的也只有首页文章**。

文章，可以抽象的理解为一个 HTML 页面，`文章` 是包括 `首页文章` 的。这个东西是为了帮助做**嵌套页面**的，例如你做了一个 Java 教程，从入门到精通，可能要好几篇博客，但是你又不想让它们直接显示在首页，而是单独在首页创建一个专门用于跳转的文章，然后在里面放上介绍，最后放上其它教程的介绍。例如下图就是一个相关的例子：

![Nested page](https://selfb.asia/images/2024/08/particles-netsted-page.webp)

(~~不要脸宣传自己博客一波~~)

其中右侧的页面就是被嵌套的页面，不会在首页中显示。

虽然使用 `Tags` 或者 `Categories` 可以达到类似的效果，但是使用这种方式对页面的控制更多。

## 构建原理

本项目实际上就是一个普通的 Next.js 项目，通过在环境变量中指向博客的目录，然后在构建时进行读取，最后借助于 Next.js 生成博客的静态文件。

如果你打算从 Hexo 博客迁移，您只需要提供下面的环境变量：

```env
BLOG_PATH=/path/to/your/hexo
```

这个环境变量的值指向你的 Hexo 博客根目录，你可以在本项目的根目录中创建一个 `.env.local` 文件，将上面的配置写入，或者你也可以直接修改 `.env` 文件，甚至如果你是 Linux 系统，你也可以通过 `export` 关键字来临时暴露这个环境变量也是可以的 (Windows 使用 `set` 命令)。

在完成上面的配置后，直接运行下面的命令就可以开始构建了：

```shell
npm run install
npm run build
```

运行完成后，会在 `out` 目录生成所有的静态文件。


# 开始使用

本博客框架最大的特点就是无侵入，你只需要拉取代码后，安装依赖并配置好相关环境变量后就可直接打包，但仍然需要你创建一个配置文件。

在你的博客根目录创建一个 `_config.yaml`，然后提供下面的配置：

```yaml
title: 博客标题
subtitle: 子标题
description: 描述
```

更多配置详见 [配置文件](/blog-config#配置文件)。

`_config.yaml` 可以提供一部分的配置，但还有另外一部分配置需要在环境变量中提供，你需要根据你的打包形式来提供相关的环境变量：


例如 [Github Pages 部署](/github-pages) 中，在 `Build` 阶段可以提供相关的[环境变量配置](/blog-config#环境变量)，例如禁用构建时的缓存：

```yaml
- name: Build
  env: 
    DATASOURCE_CACHE_ENABLE: false
  run: 'export BLOG_PATH=${GITHUB_WORKSPACE}/datasource && cd nextjs-particlex-theme && npm run build'
```

上面的代码中，我们使用了 `env` 来传递环境变量，并且关闭了构建时的缓存，除了这种方式外，在 `run` 字段中，直接通过 `export` 来设置环境变量也是可行的！例如上面的 `export BLOG_PATH=${GITHUB_WORKSPACE}/datasource`。

更多可用的环境变量可以参考：[环境变量配置](/blog-config#环境变量)。如果你有其它需要，可以根据右侧目录进行选择性观看。

---

如果你目前正在使用 hexo 作为博客框架，接下来可以直接参考 [Github Pages 部署教程](/github-pages) 来进行打包使用了。

否则，你还需要额外配置其它的环境变量后才能正常打包，可以参考：[从任意 Markdown 博客迁移](#从任意-markdown-博客迁移)。

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

## 从任意 Markdown 博客迁移

> 如果打算从 Hexo 博客迁移，则不需要配置这些内容

支持从任意 Markdown 类型的博客迁移，只要你的博客文件是由 Markdown 文件编写的，就可以进行迁移。

迁移时，除了基础的 `BLOG_PATH` 需要提供外，还需要额外提供下面三个环境变量：

- `BLOG_HOME_POST_DIRECTORY`: 首页文章文件夹
- `BLOG_RESOURCE_DIRECTORY`: 资源文件夹
- `BLOG_POST_DIRECTORY`：存放所有文章的文件夹

需要注意的是，`BLOG_HOME_POST_DIRECTORY` 可以不是 `BLOG_POST_DIRECTORY` 的子目录，它可以是任意一个单独的目录，也可以作为一个子目录。

但是！`BLOG_POST_DIRECTORY` **不能是** `BLOG_HOME_POST_DIRECTORY` 的子目录！因为在前面已经说过了，`文章` 是包含 `首页文章` 的。如果 `BLOG_POST_DIRECTORY` 是 `BLOG_HOME_POST_DIRECTORY` 的子目录，虽然首页能够正常显示所有的 `首页文章`，但是如果点击查看则会抛出 404 错误！

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

注意 `BLOG_POST_DIRECTORY` 不能为空字符串。

> 警告：由于[前面](#添加图片)说了，图片目录名称 `images`，虽然在这里指定为了 `img`，但实际访问前缀会被替换为 `images`，例如这里的 `img/coffee.png`，访问路径为 `images/coffee.png`.

## 配置 CDN

如果需要配置 CDN，你需要缓存项目中 [public](/public) 目录中所有内容到你的 CDN 中，并最终在构建阶段提供环境变量。例如在 Github Actions中：

```
- name: Build
  run: 'snip...'
  env: 
    NEXT_PUBLIC_CND_PUBLIC_PATH_BASE_URL: 'https://your.cdn/prefix'
```

> public 目录在一般情况下不会进行变动，即使变动，也不会在原有文件进行修改，所以无需担心文件内容对不上。