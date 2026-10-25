<script setup lang="ts">
// 获取 blog 下的所有文章
const { data: posts } = await useAsyncData('blog-posts', () => {
  return queryCollection('content')
    .where('path', 'LIKE', '/blog/%')
    .all()
})

// 处理日期格式和排序
const sortedPosts = computed(() => {
  if (!posts.value) return []
  return [...posts.value]
    .map(post => {
      const rawDate = post.date || post.meta?.date || post.createdAt || ''
      const formattedDate = rawDate ? String(rawDate).substring(0, 10) : '暂无日期'
      return { ...post, displayDate: formattedDate }
    })
    .sort((a, b) => {
      if (a.displayDate === '暂无日期') return 1
      if (b.displayDate === '暂无日期') return -1
      return b.displayDate.localeCompare(a.displayDate)
    })
})
</script>

<template>
  <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-8 my-8 max-w-3xl mx-auto">
    
    <!-- 标题稍微缩小一点点，用 text-3xl -->
    <h1 class="text-3xl font-bold mb-6">博客文章</h1>

    <div v-if="!sortedPosts || sortedPosts.length === 0" class="text-gray-500">
      暂时还没有文章哦~
    </div>

    <div v-else class="space-y-0">
      <div 
        v-for="post in sortedPosts" 
        :key="post.path" 
        class="flex items-center justify-between py-4 border-b border-gray-100 dark:border-gray-700 last:border-0"
      >
        <!-- 左侧标题：从 text-lg 改为 text-base，颜色用 gray-800，更像正文 -->
        <NuxtLink 
          :to="post.path" 
          class="text-base font-medium text-gray-800 dark:text-gray-200 hover:text-blue-600 dark:hover:text-blue-400 transition-colors"
        >
          {{ post.title }}
        </NuxtLink>
        
        <!-- 右侧日期：从 text-sm 改为 text-xs，颜色更淡 -->
        <span class="text-xs text-gray-400 dark:text-gray-500 whitespace-nowrap ml-4">
          {{ post.displayDate }}
        </span>
      </div>
    </div>
    
  </div>
</template>
