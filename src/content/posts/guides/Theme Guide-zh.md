---
title: 主题上手指南
published: 2025-01-26
updated: 2025-04-13
tags:
  - 博客主题
  - 指南
pin: 99
lang: zh
abbrlink: theme-guide
draft: true
---

Retypeset 是一款基于 [Astro](https://astro.build/) 框架的静态博客主题，中文名为重新编排。本文为 Retypeset 主题上手指南，主要介绍如何修改主题配置与创建新文章，来帮助你快速搭建个人博客。

## 主题配置

通过修改配置文件 [src/config.ts](https://github.com/radishzzz/astro-theme-retypeset/blob/master/src/config.ts) 来自定义你的博客。

### 站点信息

```ts
site: {
  // 站点标题
  title: 'Retypeset'
  // 站点副标题
  subtitle: 'Revive the beauty of typography'
  // 站点描述
  description: 'Retypeset is a static blog theme...'
  // 使用 src/i18n/ui.ts 中的多语言标题/副标题/站点描述，代替上方静态配置
  i18nTitle: true // true, false
  // 作者名称
  author: 'radishzz'
  // 站点地址
  url: 'https://retypeset.radishzz.cc'
  // 站点图标
  // 推荐格式: svg, png, ico
  favicon: '/icons/favicon.svg' // 或 https://example.com/favicon.svg
}
```

### 主题配色

```ts
color: {
  // 默认主题
  mode: 'light' // light, dark, auto
  // 亮色模式
  light: {
    // 高亮颜色
    // 用于站点标题、鼠标悬停效果等
    primary: 'oklch(25% 0.005 298)'
    // 次要颜色
    // 用于普通文本
    secondary: 'oklch(40% 0.005 298)'
    // 背景色
    background: 'oklch(96% 0.005 298)'
  }
  // 暗色模式
  dark: {
    // 高亮颜色
    primary: 'oklch(92% 0.005 298)'
    // 次要颜色
    secondary: 'oklch(77% 0.005 298)'
    // 背景色
    background: 'oklch(22% 0.005 298)'
  }
}
```

### 全局设置

```ts
global: {
  // 默认语言
  // 站点根路径 '/' 的语言
  locale: 'zh' // de, en, es, fr, ja, ko, pl, pt, ru, zh, zh-tw
  // 更多语言
  // 生成 '/en/' '/es/' 等多语言路径
  // 不要重复填写默认语言，可以为空 []
  moreLocales: ['en', 'es', 'ja', 'ru', 'zh-tw'] // ['de', 'en', 'es', 'fr', 'ja', 'ko', 'pl', 'pt', 'ru', 'zh', 'zh-tw']
  // 字体样式
  fontStyle: 'sans' // sans, serif
  // 文章日期格式
  // YYYY-MM-DD, MM-DD-YYYY, DD-MM-YYYY, MONTH DAY YYYY, DAY MONTH YYYY
  // 2025-04-13, 04-13-2025, 13-04-2025, Apr 13 2025，13 Apr 2025
  dateFormat: 'YYYY-MM-DD'
  // 默认为所有文章开启目录
  toc: true // true, false
  // 启用 KaTeX 数学公式渲染
  katex: true // true, false
  // 减少动画和过渡效果以提高性能
  reduceMotion: false // true, false
}
```

### 评论系统

```ts
comment: {
  // 开启评论
  enabled: true // true, false
  // waline 评论
  waline: {
    // 服务器地址
    serverURL: 'https://retypeset-comment.radishzz.cc'
    // emoji 表情地址
    emoji: [
      'https://unpkg.com/@waline/emojis@1.2.0/tw-emoji'
      // 'https://unpkg.com/@waline/emojis@1.2.0/bmoji'
      // 更多表情: https://waline.js.org/en/guide/features/emoji.html
    ]
    // gif 搜索
    search: false // true, false
    // 图片上传
    imageUploader: false // true, false
  }
}
```

### 搜索引擎优化

```ts
seo: {
  // @twitter ID
  twitterID: '@radishzz_'
  // 站点验证
  verification: {
    // google 搜索控制台
    google: 'AUCrz5F1e5qbnmKKDXl2Sf8u6y0kOpEO1wLs6HMMmlM'
    // bing 网站管理员工具
    bing: '64708CD514011A7965C84DDE1D169F87'
    // yandex 网站管理员
    yandex: ''
    // baidu 站长工具
    baidu: ''
  }
  // google 网站分析
  googleAnalyticsID: ''
  // umami 网站分析
  umamiAnalyticsID: '520af332-bfb7-4e7c-9386-5f273ee3d697'
  // follow 验证
  follow: {
    // 订阅 ID
    feedID: ''
    // 用户 ID
    userID: ''
  }
  // apiflash access key 访问密钥
  // 自动生成网站截图用于 open graph
  // 获取访问密钥: https://apiflash.com/
  apiflashKey: ''
}
```

### 自定义页脚

```ts
footer: {
  // 社交链接
  links: [
    {
      name: 'RSS',
      url: '/atom.xml', // 或 /rss.xml
    },
    {
      name: 'GitHub',
      url: 'https://github.com/radishzzz/astro-theme-retypeset',
    },
    {
      name: 'Email',
      url: 'email@radishzz.cc',
    }
    // {
    //   name: 'X',
    //   url: 'https://x.com/radishzz_',
    // },
  ]
  // 建站年份
  startYear: 2025
}
```

### 资源预加载

```ts
preload: {
  // 链接预加载策略
  linkPrefetch: 'viewport' // hover, tap, viewport, load
  // 图床地址
  // 优化 Markdown 文件中的远程图片以避免布局抖动
  imageHostURL: 'https://image.radishzz.cc'
  // 定制 google analytics js
  // 适用于路由 google analytics js 到自定义域名的用户
  customGoogleAnalyticsJS: ''
  // 定制 umami analytics js
  // 适用于自部署 umami，或路由 umami analytics js 到自定义域名的用户
  customUmamiAnalyticsJS: 'https://js.radishzz.cc/jquery.min.js'
}
```

## 更多配置

除了配置文件 `src/config.ts` 外，还有部分配置项位于其它文件中。

### 语法高亮

代码块的语法高亮主题。

```ts
// astro.config.ts

shikiConfig: {
  // 可选主题：https://shiki.style/themes
  // 背景色固定跟随博客主题，而非语法高亮主题
  themes: {
    light: 'github-light' // 亮色主题
    dark: 'github-dark' // 暗色主题
  }
}
```

### 文章摘要

文章自动摘要的字符数量。

```ts
// src/utils/description.ts

const EXCERPT_LENGTHS: Record<ExcerptScene, {
  cjk: number // 中文、日文、韩文
  other: number // 其他语言
}> = {
  list: { // 首页文章列表
    cjk: 120, // 自动摘要前 120 字
    other: 240, // 自动摘要前 240 字
  },
}
```

### Open Graph

[Open Graph 社交卡片](https://orcascan.com/tools/open-graph-validator?url=https%3A%2F%2Fretypeset.radishzz.cc%2Fposts%2Ftheme-guide%2F) 样式。

```ts
// src/pages/og/[...image].ts

getImageOptions: (_path, page) => ({
  logo: {
    path: './public/icons/og-logo.png', // 本地路径的 PNG 图片
    size: [250], // logo 宽度
  },
  font: {
    title: { // 标题
      families: ['Noto Sans SC'], // 字体
      weight: 'Bold', // 字重
      color: [34, 33, 36], // 颜色
      lineHeight: 1.5, // 行高
    },
  },
  fonts: [ // 字体路径（本地或远程）
    'https://cdn.jsdelivr.net/gh/notofonts/noto-cjk@main/Sans/SubsetOTF/SC/NotoSansSC-Bold.otf',
    'https://cdn.jsdelivr.net/gh/notofonts/noto-cjk@main/Sans/SubsetOTF/SC/NotoSansSC-Regular.otf',
  ],
  bgGradient: [[242, 241, 245]], // 背景色
  // 更多配置：https://github.com/delucis/astro-og-canvas/tree/latest/packages/astro-og-canvas
})
```

### RSS 订阅

[RSS 订阅页](https://retypeset.radishzz.cc/rss.xml) 配色。

```html
<!-- public/feeds/xxx-style.xsl -->

<style type="text/css">
body{color:oklch(25% 0.005 298)} /* 字体颜色 */
.bg-white{background-color:oklch(0.96 0.005 298)!important} /* 背景颜色 */
.text-gray{color:oklch(0.25 0.005 298 / 75%)!important} /* 次要字体颜色 */
</style>
```

## 创建新文章

执行 `pnpm new-post <filename>` 创建新文章，可在 `src/content/posts/` 目录中编辑。

```bash
pnpm new-post                      ->  src/content/posts/new-post.md
pnpm new-post first-post           ->  src/content/posts/first-post.md
pnpm new-post 2025/03/first-post   ->  src/content/posts/2025/03/first-post.md
pnpm new-post first-post.mdx       ->  src/content/posts/first-post.mdx
```

### Front Matter

`title` 和 `published` 为必填项，其余配置均可安全删除。

```md
---
# 必填
title: 主题上手指南
published: 2025-01-26

# 可选
description: 自动选取文章前 120 字作为摘要。
updated: 2025-03-26
tags:
  - 博客主题
  - 指南

# 进阶，可选
draft: true/false
pin: 0-99
toc: true/false
lang: de/en/es/fr/ja/ko/pl/pt/ru/zh/zh-tw
abbrlink: theme-guide
---
```

### 进阶配置

#### draft

是否标记文章为草稿。设为 true 时无法发布文章，仅供本地开发预览。默认为 false。

#### pin

是否置顶文章。数字越大，文章的置顶优先级越高。默认为 0，即不置顶。

#### toc

是否生成目录。显示 h2 至 h4 标题。默认由全局配置项 `global.toc` 决定，可在文章中单独设置以覆盖全局配置。

#### lang

指定文章语言。只能指定一种语言，不指定则默认显示在所有语言路径下。

```md
# src/config.ts
# locale: 'en'
# moreLocales: ['es', 'ru']

# lang: ''
src/content/posts/apple.md   ->  example.com/posts/apple/
                             ->  example.com/es/posts/apple/
                             ->  example.com/ru/posts/apple/
# lang: en
src/content/posts/apple.md   ->  example.com/posts/apple/
# lang: es
src/content/posts/apple.md   ->  example.com/es/posts/apple/
# lang: ru
src/content/posts/apple.md   ->  example.com/ru/posts/apple/
```

#### abbrlink

自定义文章 URL。只能包含小写字母、数字和连字符 `-`。

```md
# src/config.ts
# locale: 'en'
# moreLocales: ['es', 'ru']
# lang: 'es'

# abbrlink: ''
src/content/posts/apple.md           ->  example.com/es/posts/apple/
src/content/posts/guide/apple.md     ->  example.com/es/posts/guide/apple/
src/content/posts/2025/03/apple.md   ->  example.com/es/posts/2025/03/apple/

# abbrlink: 'banana'
src/content/posts/apple.md           ->  example.com/es/posts/banana/
src/content/posts/guide/apple.md     ->  example.com/es/posts/banana/
src/content/posts/2025/03/apple.md   ->  example.com/es/posts/banana/
```
