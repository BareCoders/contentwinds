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
const pageSize = 10 // 每页显示的文章数量，你可以改成 5 或 10

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
  <!-- 核心修改：统一为磨砂半透明卡片，加上 mt-24 拉开与导航栏的距离 -->
  <div class="bg-white/30 dark:bg-[#191a1c]/30 backdrop-blur-md ring-1 ring-white/40 dark:ring-gray-800 w-full max-w-4xl mx-auto px-4 sm:px-8 py-6 sm:py-8 rounded-2xl shadow-sm mt-4 mb-8 min-h-[calc(100vh-100px)]">
    
    <h1 class="text-3xl font-bold mb-6 text-[#1e293b] dark:text-white">博客文章</h1>

    <div v-if="!paginatedPosts || paginatedPosts.length === 0" class="text-gray-500">
      暂时还没有文章哦~
    </div>

    <div v-else>
      <!-- 文章列表区域 -->
      <div class="space-y-0">
        <div 
          v-for="post in paginatedPosts" 
          :key="post.path" 
          class="flex items-center justify-between py-4 border-b border-white/50 dark:border-gray-700/50 last:border-0"
        >
          <!-- 标题颜色调深，悬停变为宝蓝色 -->
          <NuxtLink 
            :to="post.path" 
            class="text-base font-medium text-[#334155] dark:text-gray-200 hover:text-[#2563eb] dark:hover:text-blue-400 transition-colors"
          >
            {{ post.title }}
          </NuxtLink>
          
          <span class="text-xs text-gray-500 dark:text-gray-400 whitespace-nowrap ml-4">
            {{ post.displayDate }}
          </span>
        </div>
      </div>

      <!-- 翻页按钮区域 -->
      <div class="flex items-center justify-between mt-8 pt-4 border-t border-white/50 dark:border-gray-700/50">
        <button 
          @click="prevPage" 
          :disabled="currentPage === 1"
          class="px-4 py-2 text-sm font-medium text-gray-700 bg-white/50 rounded-lg hover:bg-white/80 disabled:opacity-50 disabled:cursor-not-allowed dark:bg-gray-700/50 dark:text-gray-300 dark:hover:bg-gray-600/50 transition-colors shadow-sm"
        >
          上一页
        </button>
        
        <span class="text-sm text-gray-600 dark:text-gray-400">
          第 {{ currentPage }} / {{ totalPages }} 页
        </span>

        <button 
          @click="nextPage" 
          :disabled="currentPage === totalPages"
          class="px-4 py-2 text-sm font-medium text-gray-700 bg-white/50 rounded-lg hover:bg-white/80 disabled:opacity-50 disabled:cursor-not-allowed dark:bg-gray-700/50 dark:text-gray-300 dark:hover:bg-gray-600/50 transition-colors shadow-sm"
        >
          下一页
        </button>
      </div>
    </div>
    
  </div>
</template>
