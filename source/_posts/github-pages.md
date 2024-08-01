---
title: Github Pages 部署教程
---

# 使用 Actions 部署

在项目根目录创建 `.github/workflows/pages.yaml`，添加如下内容：

```yaml
name: Pages

on:
  push:
    branches:
      - master 

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:

    - name: Checkout Main Repository 
      uses: actions/checkout@v4
      with:
        repository: IceOfSummer/nextjs-particlex-theme
        # 这里可以选一个 tag 或者 分支
        ref: master
        path: nextjs-particlex-theme

    - name: Cache Next.js
      uses: actions/cache@v4
      with:
        path: nextjs-particlex-theme/.next/cache
        key: nextjs-cache

    - name: Use Node.js 20
      uses: actions/setup-node@v4
      with:
        node-version: "20"
        cache: 'npm'
        cache-dependency-path: nextjs-particlex-theme/package-lock.json

    - name: Checkout Datasource Repository
      uses: actions/checkout@v4
      with:
        path: datasource

    - name: Install Dependencies
      run: 'cd nextjs-particlex-theme && npm install'

    - name: Build
      run: 'export BLOG_PATH=${GITHUB_WORKSPACE}/datasource && cd nextjs-particlex-theme && npm run build'

    - name: Upload GitHub Pages artifact
      uses: actions/upload-pages-artifact@v3
      with:
        path: 'nextjs-particlex-theme/out'

  
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
    
```

## 配置 CDN

如果需要配置 CDN，你需要缓存项目中 `public` 目录中所有内容到你的 CDN 中，并最终在 `Build` 阶段提供环境变量：

```
- name: Build
  run: 'export BLOG_PATH=${GITHUB_WORKSPACE}/datasource && cd        nextjs-particlex-theme && npm run build'
  env: 
    NEXT_PUBLIC_CND_PUBLIC_PATH_BASE_URL: 'https://your.cdn/prefix'
```

