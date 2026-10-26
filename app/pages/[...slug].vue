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
  <!-- 
    核心修改说明：
    1. mt-4：顶部间距再次减半，几乎贴近导航栏，紧凑感拉满。
    2. max-w-4xl mx-auto px-4：限制卡片最大宽度为 896px（最适合阅读的宽度），并在左右留出安全的间距。
    3. min-h-[calc(100vh-100px)]：保持卡片撑满屏幕高度。
  -->
  <NuxtLayout :name="page?.layout as LayoutKey || 'default'" class="bg-white/30 dark:bg-[#191a1c]/30 backdrop-blur-md ring-1 ring-white/40 dark:ring-gray-800 w-full max-w-4xl mx-auto px-4 sm:px-8 py-6 sm:py-8 rounded-2xl shadow-sm mt-4 mb-8 min-h-[calc(100vh-100px)]">
    
    <div class="flex flex-col lg:flex-row gap-6 lg:gap-8 items-start w-full">
      
      <!-- 左侧大纲：电脑端显示并吸顶，手机端隐藏 -->
      <aside class="hidden lg:block w-48 shrink-0 sticky top-24">
        <div class="text-sm font-bold text-gray-500 mb-4 uppercase tracking-wider">目录大纲</div>
        <div class="border-l-2 border-blue-200/50 dark:border-gray-800 pl-4 space-y-2">
          <a 
            v-for="link in tocLinks" 
            :key="link.id"
            :href="`#${link.id}`" 
            class="block text-gray-600 hover:text-[#2563eb] dark:text-gray-400 dark:hover:text-[#9fbcd0] transition-colors py-1 text-sm" 
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
