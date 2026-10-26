<script setup lang="ts">
import type { LayoutKey } from '#build/types/layouts'
import { computed } from 'vue'

const route = useRoute()

const { data: page } = await useAsyncData(`page-${route.params.slug}`, () => {
  return queryCollection('content').path(route.path).first()
})

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found' })
}

useSeoMeta(page.value?.seo || {})
const isBlogPage = computed(() => route.path.startsWith('/blog/'))

// 自己抓取大纲
const tocLinks = computed(() => {
  const links = page.value?.body?.toc?.links || page.value?.toc?.links || []
  return links.filter((link: any) => link.depth > 1)
})
</script>

<template>
  <!-- 
    mt-24 解决顶部遮挡问题，同时不会空太多。
    prose-pre 颜色加深，解决代码块白底看不清字的问题。
  -->
  <NuxtLayout :name="page?.layout as LayoutKey || 'default'" class="bg-white dark:bg-[#191a1c] ring-1 ring-gray-200 dark:ring-gray-700 w-full p-4 sm:p-8 rounded-lg shadow mt-24 mb-8">
    
    <div class="flex flex-col lg:flex-row gap-6 lg:gap-8 items-start w-full overflow-hidden">
      
      <!-- 左侧大纲 -->
      <aside class="hidden lg:block w-56 shrink-0 sticky top-24">
        <div class="text-sm font-bold text-gray-500 mb-4 uppercase tracking-wider">目录大纲</div>
        <div class="border-l-2 border-gray-100 dark:border-gray-800 pl-4 space-y-2">
          <a 
            v-for="link in tocLinks" 
            :key="link.id"
            :href="`#${link.id}`" 
            class="block text-gray-600 hover:text-[#46677e] dark:text-gray-400 dark:hover:text-[#9fbcd0] transition-colors py-1 text-sm" 
            :class="{ 'pl-4': link.depth === 3 }"
          >
            {{ link.text }}
          </a>
        </div>
      </aside>

      <!-- 右侧正文 -->
      <div class="flex-1 w-full min-w-0">
        <!-- 这里修改了 prose-pre:bg-gray-100 为更深的颜色，修复代码块发白 -->
        <ContentRenderer
          v-if="page"
          :value="page"
          class="prose dark:prose-invert prose-pre:bg-gray-50 dark:prose-pre:bg-gray-800 max-w-none break-words text-sm sm:text-base"
        />
        
        <Comments v-if="isBlogPage" />
      </div>
      
    </div>
  </NuxtLayout>
</template>
