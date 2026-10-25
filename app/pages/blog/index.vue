<script setup lang="ts">
// 自动获取 content/blog 目录下的所有文章，并按日期从新到旧排序
const { data: posts } = await useAsyncData('blog-posts', () => {
  return queryContent('/blog').sort({ date: -1 }).find()
})
</script>

<template>
  <!-- 外层加个带背景和圆角的卡片容器，跟首页风格保持一致 -->
  <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-8 my-8 max-w-3xl mx-auto">
    
    <h1 class="text-4xl font-bold mb-8">博客文章</h1>

    <!-- 如果没文章，显示提示 -->
    <div v-if="!posts || posts.length === 0" class="text-gray-500">
      暂时还没有文章哦~
    </div>

    <!-- 文章列表区域 -->
    <div v-else class="space-y-6">
      <!-- 循环渲染每一篇文章 -->
      <div 
        v-for="post in posts" 
        :key="post._path" 
        class="flex items-center justify-between border-b pb-4 last:border-0 dark:border-gray-700"
      >
        <!-- 左侧：标题（链接） -->
        <NuxtLink 
          :to="post._path" 
          class="text-xl font-semibold text-blue-600 dark:text-blue-400 hover:underline"
        >
          {{ post.title }}
        </NuxtLink>
        
        <!-- 右侧：日期（自动读取 md 文件头部的 date） -->
        <span class="text-sm text-gray-500 dark:text-gray-400 whitespace-nowrap ml-4">
          {{ post.date || '暂无日期' }}
        </span>
      </div>
    </div>
    
  </div>
</template>
