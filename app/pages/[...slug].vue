<script setup lang="ts">
import type { LayoutKey } from '#build/types/layouts'

const route = useRoute()

const { data: page } = await useAsyncData(`page-${route.params.slug}`, () => {
  return queryCollection('content').path(route.path).first()
})

if (!page.value) {
  throw createError({
    statusCode: 404,
    statusMessage: 'Page not found',
  })
}

useSeoMeta(page.value?.seo || {})

// 👇 新增：判断当前页面路径是否以 /blog/ 开头
const isBlogPage = computed(() => route.path.startsWith('/blog/'))
</script>

<template>
  <NuxtLayout :name="page?.layout as LayoutKey || 'default'" class="bg-white dark:bg-gray-800 ring-1 ring-gray-200 dark:ring-gray-700 w-full p-8 rounded-lg shadow my-8">
    <ContentRenderer
      v-if="page"
      :value="page"
      class="prose dark:prose-invert prose-pre:bg-gray-100 dark:prose-pre:bg-gray-900 max-w-none"
    />
    
    <!-- 👇 加上 v-if="isBlogPage"，只有博客文章才显示评论 -->
    <Comments v-if="isBlogPage" />
    
  </NuxtLayout>
</template>
