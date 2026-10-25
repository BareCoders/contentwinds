<script setup lang="ts">
// 1. 获取 blog 下的所有文章
const { data: posts } = await useAsyncData('blog-posts', () => {
  return queryCollection('content')
    .where('path', 'LIKE', '/blog/%')
    .all()
})

// 2. 处理日期格式和排序
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

// ================= 分页逻辑 =================
const currentPage = ref(1)
const pageSize = 3 // 每页显示的文章数量，你可以改成 5 或 10

// 计算当前页应该显示哪些文章
const paginatedPosts = computed(() => {
  const start = (currentPage.value - 1) * pageSize
  const end = start + pageSize
  return sortedPosts.value.slice(start, end)
})

// 计算总页数
const totalPages = computed(() => {
  return Math.ceil(sortedPosts.value.length / pageSize)
})

// 翻页函数
const nextPage = () => {
  if (currentPage.value < totalPages.value) currentPage.value++
}
const prevPage = () => {
  if (currentPage.value > 1) currentPage.value--
}
// ============================================
</script>

<template>
  <!-- 把 max-w-3xl 改成了 w-full，让卡片撑满父容器的宽度 -->
  <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-8 my-8 w-full">
    
    <h1 class="text-3xl font-bold mb-6">博客文章</h1>

    <div v-if="!paginatedPosts || paginatedPosts.length === 0" class="text-gray-500">
      暂时还没有文章哦~
    </div>

    <div v-else>
      <!-- 文章列表区域 -->
      <div class="space-y-0">
        <div 
          v-for="post in paginatedPosts" 
          :key="post.path" 
          class="flex items-center justify-between py-4 border-b border-gray-100 dark:border-gray-700 last:border-0"
        >
          <NuxtLink 
            :to="post.path" 
            class="text-base font-medium text-gray-800 dark:text-gray-200 hover:text-blue-600 dark:hover:text-blue-400 transition-colors"
          >
            {{ post.title }}
          </NuxtLink>
          
          <span class="text-xs text-gray-400 dark:text-gray-500 whitespace-nowrap ml-4">
            {{ post.displayDate }}
          </span>
        </div>
      </div>

      <!-- 翻页按钮区域 -->
      <div class="flex items-center justify-between mt-8 pt-4 border-t border-gray-100 dark:border-gray-700">
        <button 
          @click="prevPage" 
          :disabled="currentPage === 1"
          class="px-4 py-2 text-sm font-medium text-gray-600 bg-gray-100 rounded hover:bg-gray-200 disabled:opacity-50 disabled:cursor-not-allowed dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600 transition-colors"
        >
          上一页
        </button>
        
        <span class="text-sm text-gray-500">
          第 {{ currentPage }} / {{ totalPages }} 页
        </span>

        <button 
          @click="nextPage" 
          :disabled="currentPage === totalPages"
          class="px-4 py-2 text-sm font-medium text-gray-600 bg-gray-100 rounded hover:bg-gray-200 disabled:opacity-50 disabled:cursor-not-allowed dark:bg-gray-700 dark:text-gray-300 dark:hover:bg-gray-600 transition-colors"
        >
          下一页
        </button>
      </div>
    </div>
    
  </div>
</template>
