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
    核心修改：实现了浅蓝底色上的半透明磨砂卡片
    1. bg-white/60：60% 透明度的白色，露出底部的浅蓝色
    2. backdrop-blur-md：磨砂毛玻璃效果
    3. ring-white/50：非常细微的白色反光边框
    4. rounded-2xl shadow-sm：大圆角和极其柔和的阴影
  -->
  <NuxtLayout :name="page?.layout as LayoutKey || 'default'" class="bg-white/60 dark:bg-[#191a1c]/60 backdrop-blur-md ring-1 ring-white/50 dark:ring-gray-800 w-full p-4 sm:p-8 rounded-2xl shadow-sm mt-24 mb-8">
    
    <div class="flex flex-col lg:flex-row gap-6 lg:gap-8 items-start w-full overflow-hidden">
      
      <!-- 左侧大纲 -->
      <aside class="hidden lg:block w-56 shrink-0 sticky top-24">
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

      <!-- 右侧正文 -->
      <div class="flex-1 w-full min-w-0">
        <ContentRenderer
          v-if="page"
          :value="page"
          class="prose dark:prose-invert max-w-none break-words text-sm sm:text-base"
        />
        
        <Comments v-if="isBlogPage" />
      </div>
      
    </div>
  </NuxtLayout>
</template>
