---
title: 更新日志：从零部署到极简冰蓝导航站
description: 记录本站从基础模板蜕变为冰蓝磨砂风格的全过程，以及在 Cloudflare 部署时踩过的坑。
date: 2026-10-01
---

# 更新日志：从零部署到极简导航站

> **💡 主题来源**：本站基于 **Content Wind** 主题构建。
> - 原作者：Atinux (Nuxt 核心团队成员)
> - 原版仓库：[https://github.com/Atinux/content-wind](https://github.com/Atinux/content-wind)
> - 本站源码：[https://github.com/BareCoders/contentwinds](https://github.com/BareCoders/contentwinds)

这个站点从最初的纯白模板，一步步变成了现在的冰蓝色极简风。这里记录一下搭建过程中的关键修改，以及部署时遇到的“坑”，权当是个备忘录。

## 🛠️ 核心功能改造

### 1. 添加 Blog 分类与分页
原版主题只有简单的文档页面，为了做导航站，我给它加上了专门的博客列表。
- 编写了 `app/pages/blog/index.vue`，实现了文章列表的展示与分页（每页 10 篇）。
- 给文章详情页（`[...slug].vue`）加上了自定义大纲，并且做了判断：只有在电脑端才显示侧边栏大纲。

### 2. 彻底解放页面宽度
原版主题为了纯粹的阅读体验，把宽度限制得很死（`max-w-2xl`）。我觉得看着太窄，于是开始“暴力”修改：
- 把 `app.vue` 里的整体容器改成了 `w-[80%]`。
- 去 `layouts/default.vue` 和 `layouts/page.vue` 里，把限制宽度的 `max-w-2xl` 统统删掉，换成了 `w-full`。
- 最终，页面成功撑开，大屏幕看起来舒服多了。

### 3. UI 终极进化：冰蓝磨砂与虚线美学
这是耗时最久、也是改动最彻底的阶段：
- **全局配色**：去掉了死板的纯白底色，换成了护眼的**浅冰蓝色 (`#dbeafe`)**，文章卡片使用半透明的**磨砂毛玻璃**（`bg-white/30` + `backdrop-blur`），透出微弱的高级色差。
- **代码块修复**：之前暗黑模式下代码块是白底白字，完全看不清。通过修改 `main.css`，硬核锁定了深色模式下的**黑底白字**样式。
- **全站虚线**：将文章列表、分页按钮上方、以及评论区上方的分割线，全部换成了极细的浅灰色**虚线**，消除了生硬的割裂感。

### 4. 评论与交互优化
- **Giscus 评论**：接入了基于 GitHub Discussions 的评论系统，并直接拉满了适配度。浅色模式下剔除了极其突兀的黑色粗边框，换成了无边框的透明主题，完美融入背景。
- **外链优化**：在 `app.vue` 写了一段全局监听脚本，现在点开文章里的外部链接（比如那些域名注册工具），都会**自动在新窗口打开**，防止访客走丢。

---

## 🚀 Cloudflare 部署踩坑记录

把代码推到 Cloudflare 的时候，真是一波三折，甚至中途换了部署方案：

### 坑一：Worker 版本限制
一开始试图用 Cloudflare 的 Worker 服务，并在部署命令里用了 `npx wrangler deploy`。
**结果**：直接报错，提示 `The version of Nuxt used in the project ("3.15.4") cannot be automatically configured`。
**原因**：Cloudflare Workers 对 Nuxt 版本有硬性要求，旧版本直接罢工。

### 坑二：找不到输出目录
放弃了 Workers，转而使用 Cloudflare Pages，连上了 Git 仓库。
- 把构建命令从 `pnpm run build` 改成 `pnpm run generate`。
- 构建成功了，但死活提示找不到 `.output/public` 目录。
**解决**：去设置里，把构建输出目录改成了 **`dist`**，一次点亮。

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

### 坑四：Tailwind CSS 编译报错
在折腾暗黑模式颜色时，试图在 `@apply` 里面直接写 `!important`，导致 Tailwind v4 编译器直接报错罢工。
**解决**：删除了 CSS 里的 `!important` 单词，改用层级选择器覆盖。

---

## 🎯 总结

现在这个网站已经非常符合我的预期：极简、快速、干净，有着独一无二的冰蓝色磨砂质感。想加什么书签或者文章，直接在 GitHub 里建个 `.md` 文件推上去就行。这才是独立建站最大的乐趣。
