<script setup lang="ts">
// 1. 精准过滤：只查 blog 文件夹下的文章，排除掉首页和About
const { data: posts } = await useAsyncData('blog-posts', () => {
  return queryCollection('content')
    .where('path', 'LIKE', '/blog/%')
    .all()
})

// 2. 手动处理数据：转换日期格式，并按时间倒序排列（新的在上面）
const sortedPosts = computed(() => {
  if (!posts.value) return []
  return [...posts.value]
    .map(post => {
      // Nuxt v3 可能把日期存在不同的字段里
      const rawDate = post.date || post.meta?.date || post.createdAt || ''
      // 如果是时间戳或长字符串，截取前10位变成 YYYY-MM-DD
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
    
    <h1 class="text-4xl font-bold mb-8">博客文章</h1>

    <div v-if="!sortedPosts || sortedPosts.length === 0" class="text-gray-500">
      暂时还没有文章哦~
    </div>

    <div v-else class="space-y-0">
      <!-- 3. 优化列表样式：去掉粗线条，改用极细的边框，字号调小 -->
      <div 
        v-for="post in sortedPosts" 
        :key="post.path" 
        class="flex items-center justify-between py-4 border-b border-gray-100 dark:border-gray-700 last:border-0"
      >
        <!-- 左侧：标题（字号改成 text-lg，看起来更精致） -->
        <NuxtLink 
          :to="post.path" 
          class="text-lg text-gray-700 dark:text-gray-300 hover:text-blue-600 dark:hover:text-blue-400 transition-colors"
        >
          {{ post.title }}
        </NuxtLink>
        
        <!-- 右侧：日期（缩小字号，颜色调淡） -->
        <span class="text-sm text-gray-400 dark:text-gray-500 whitespace-nowrap ml-4">
          {{ post.displayDate }}
        </span>
      </div>
    </div>
    
  </div>
</template>
