<script setup lang="ts">
import type { LayoutKey } from '#build/types/layouts'
import { computed } from 'vue'

const route = useRoute()

// 获取当前页面内容
const { data: page } = await useAsyncData(`page-${route.params.slug}`, () => {
  return queryCollection('content').path(route.path).first()
})

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found' })
}

useSeoMeta(page.value?.seo || {})

// 判断是否为博客文章（用于决定是否显示评论区）
const isBlogPage = computed(() => route.path.startsWith('/blog/'))

// 抓取文章大纲：过滤掉 H1 大标题，只保留 H2 和 H3
const tocLinks = computed(() => {
  const links = page.value?.body?.toc?.links || page.value?.toc?.links || []
  return links.filter((link: any) => link.depth > 1)
})
</script>

<template>
  <!-- 核心瘦身：直接调用 card-glass 即可，宽高背景全自动适配 -->
  <NuxtLayout :name="page?.layout as LayoutKey || 'default'" class="card-glass">
    
    <div class="flex flex-col lg:flex-row gap-6 lg:gap-8 items-start w-full">
      
      <!-- 左侧大纲：电脑端显示并吸顶，手机端隐藏 -->
      <aside class="hidden lg:block w-56 shrink-0 sticky top-24">
        <div class="text-sm font-bold text-theme-text dark:text-dark-text mb-4 uppercase tracking-wider">目录大纲</div>
        <!-- 使用主题边框颜色 -->
        <div class="border-l-2 border-theme-border/60 dark:border-dark-border pl-4 space-y-2">
          <a 
            v-for="link in tocLinks" 
            :key="link.id"
            :href="`#${link.id}`" 
            class="block text-theme-text hover:text-[#2563eb] dark:text-dark-text dark:hover:text-dark-link transition-colors py-1 text-sm" 
            :class="{ 'pl-4': link.depth === 3 }"
          >
            {{ link.text }}
          </a>
        </div>
      </aside>

      <!-- 右侧正文内容 -->
      <div class="flex-1 w-full min-w-0">
        <ContentRenderer
          v-if="page"
          :value="page"
          class="prose dark:prose-invert max-w-none break-words text-sm sm:text-base"
        />
        
        <!-- 只有博客文章才显示评论组件 -->
        <Comments v-if="isBlogPage" />
      </div>
      
    </div>
  </NuxtLayout>
</template>
