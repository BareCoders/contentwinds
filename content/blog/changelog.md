---
title: 更新日志：从零部署到极简导航站
description: 记录本站添加 Blog 分类、修改布局宽度以及在使用 Cloudflare 部署时踩过的坑。
date: 2026-10-26
---

# 更新日志：从零部署到极简导航站

> **💡 主题来源**：本站基于 **Content Wind** 主题构建。
> - 原作者：Atinux (Nuxt 核心团队成员)
> - 原版仓库：[https://github.com/Atinux/content-wind](https://github.com/Atinux/content-wind)
> - 本站源码：[https://github.com/BareCoders/contentwinds](https://github.com/BareCoders/contentwinds)

这个站点从最初的模板，一步步变成了现在的样子。这里记录一下搭建过程中的关键修改，以及部署时遇到的“坑”，权当是个备忘录。

## 🛠️ 核心修改记录

### 1. 添加 Blog 分类与分页
原版主题只有简单的页面，我给它加上了专门的博客列表。
- 编写了 `app/pages/blog/index.vue`，实现了文章列表的展示。
- 加入了**分页功能**（每页 3 篇），并且在手机端做了横向自适应的排版。
- 给文章详情页（`[...slug].vue`）加上了 Giscus 评论系统，而且做了判断：只有博客文章才显示评论，独立页面（如 About）不显示。

### 2. 修改布局宽度
原版主题为了纯粹的阅读体验，把宽度限制得很死（`max-w-2xl`）。我一直觉得看着太窄，于是开始“暴力”修改：
- 把 `app.vue` 里的整体容器改成了 `w-[80%]`。
- 去 `layouts/default.vue` 和 `layouts/page.vue` 里，把限制宽度的 `max-w-2xl` 统统删掉，换成了 `w-full`。
- 最终，页面成功撑开，大屏幕看起来舒服多了。

### 3. 关于颜色
说实话，我一直没去改主题底层的颜色变量，目前的黑字白底（以及暗黑模式）已经足够耐看，就不瞎折腾了。

---

## 🚀 Cloudflare 部署踩坑与版本调整

把代码推到 Cloudflare 的时候，真是一波三折，甚至中途换了部署方案：

### 版本一：尝试使用 Cloudflare Workers（失败）
一开始试图用 Cloudflare 的 Worker 服务，并在部署命令里用了 `npx wrangler deploy`。
**结果**：直接报错，提示 `The version of Nuxt used in the project ("3.15.4") cannot be automatically configured. Please update the Nuxt version to at least "3.21.0"`。
**原因**：Cloudflare Workers 的自动配置功能对 Nuxt 版本有硬性要求，旧版本直接罢工。

### 版本二：改用 Cloudflare Pages（成功）
放弃了 Workers，转而使用专门托管静态前端的 Cloudflare Pages。
**解决**：
1. 重新连接 Git 仓库。
2. 把构建命令从 `pnpm run build` 改成 `pnpm run generate`（绕开服务端渲染，直接生成纯静态 HTML）。
3. 构建成功了，但死活提示找不到 `.output/public` 目录。最后去设置里，把构建输出目录改成了 **`dist`**，一次点亮。

### 坑三：死链导致构建崩溃（最折磨）
部署成功后，删除了几篇不需要的默认文章，再次提交代码时，构建死活不通过，报错全是 `[404] Page not found`。
**原因**：首页或者其他文章里，还残留着指向那些被删页面的链接。Nuxt 在静态生成时会像爬虫一样去访问这些链接，一旦发现 404，整个构建直接罢工。
**解决**：在 `nuxt.config.ts` 里加上了“免死金牌”：
```javascript
nitro: {
  prerender: {
    failOnError: false
  }
}
