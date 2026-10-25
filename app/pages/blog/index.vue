<script setup lang="ts">
// 最暴力的查法：把整个 'content' 集合里的所有东西全拉出来
const { data: posts } = await useAsyncData('blog-posts', () => {
  return queryCollection('content').all()
})
</script>

<template>
  <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-8 my-8 max-w-3xl mx-auto">
    <h1 class="text-4xl font-bold mb-8">博客文章</h1>

    <!-- 打印一下它到底抓到了多少个东西 -->
    <p class="mb-4 text-red-500 font-bold">
      系统抓取到的文件总数: {{ posts?.length || 0 }} 个
    </p>

    <div v-if="!posts || posts.length === 0" class="text-gray-500">
      依然没有文章，可能是 md 文件解析失败。
    </div>

    <div v-else class="space-y-6">
      <div 
        v-for="post in posts" 
        :key="post.path" 
        class="flex items-center justify-between border-b pb-4 last:border-0 dark:border-gray-700"
      >
        <!-- 用可选链防止报错 -->
        <NuxtLink 
          :to="post.path || '#'" 
          class="text-xl font-semibold text-blue-600 dark:text-blue-400 hover:underline"
        >
          {{ post.title || '无标题' }}
        </NuxtLink>
        
        <span class="text-sm text-gray-500 dark:text-gray-400 whitespace-nowrap ml-4">
          {{ post.date || '暂无日期' }}
        </span>
      </div>
    </div>
  </div>
</template>
