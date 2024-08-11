---
title: 进阶操作
date: 2024/08/11 17:26:51
tags: 进阶
seo:
	description: particlex 主题的进阶操作。
---

# 配置 CDN

首先将项目中 [/public](/public) 目录中的所有文件上传到你的 CDN 中。

例如 CDN 访问地址为 `https://foo.bar.com`，此时将 `public/` 目录中的**所有**文件上传到 `/static/particlex/` 目录中。


最后将环境变量 `NEXT_PUBLIC_CND_PUBLIC_PATH_BASE_URL` 的值设置为 `https://foo.bar.com/static/particlex` 即可。

> 我们几乎不会更新 `/public` 目录中的东西，所以无需担心在一个大版本中文件对不上。

# 添加图片

> **强烈建议使用 CDN 保存你的图片，而不是直接和博客静态文件一起分发。**

由于 next.js 限制 (其实有解，但是不太优雅)，所有的图片必须放到 `images` 目录中。

例如在 hexo 中，图片必须全部放到 `source/images` 中，**放在其它任何地方的图片都会被忽略**。

---

所有的图片都使用了原生懒加载 `loading="_lazy"`。

## 设置网站图标

对于网站图标 `favicon.ico` 可以直接放在根目录中。

例如在 hexo 中需要放在 `source/favicon.ico` 中。

# 小心使用 > 和 <

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

# 配置评论组件

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

# 配置 Github Pages

[Github Pages 部署教程](/github-pages)