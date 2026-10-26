# 书签与建站导航站

本项目是一个基于 **Nuxt 3** 和 **Nuxt Content** 搭建的极简个人导航与书签站点。主要使用 Markdown 来管理和编写内容。

## 🛠️ 技术栈
- 框架：Nuxt 3 (Vue 3)
- 样式：Tailwind CSS
- 内容引擎：Nuxt Content
- 托管平台：Cloudflare Pages
- 评论系统：Giscus

---

## 🚀 核心修改指南

### 1. 博客首页每页显示几篇文章在哪里修改？
默认每页显示 3 篇文章。修改方法：
1. 打开 `app/pages/blog/index.vue` 文件。
2. 在 `<script setup>` 标签里，找到代码：
   ```javascript
   const pageSize = 3
