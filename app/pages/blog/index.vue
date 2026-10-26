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
const pageSize = 10 // 每页显示的文章数量

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
  <!-- 核心瘦身：直接调用 card-glass -->
  <div class="card-glass">
    
    <h1 class="text-3xl font-bold mb-6 text-theme-title dark:text-dark-title">博客文章</h1>

    <div v-if="!paginatedPosts || paginatedPosts.length === 0" class="text-theme-text dark:text-dark-text">
      暂时还没有文章哦~
    </div>

    <div v-else>
      <!-- 文章列表区域 -->
      <div class="space-y-0">
        <div 
          v-for="post in paginatedPosts" 
          :key="post.path" 
          class="flex items-center justify-between py-4 border-b divider-dashed last:border-0"
        >
          <!-- 标题颜色使用主题变量 -->
          <NuxtLink 
            :to="post.path" 
            class="text-base font-medium text-theme-title dark:text-dark-title hover:text-blue-600 dark:hover:text-dark-link transition-colors"
          >
            {{ post.title }}
          </NuxtLink>
          
          <span class="text-xs text-theme-text dark:text-dark-text whitespace-nowrap ml-4">
            {{ post.displayDate }}
          </span>
        </div>
      </div>

      <!-- 翻页按钮区域 -->
      <div class="flex items-center justify-between mt-8 pt-4 border-t divider-dashed">
        <button 
          @click="prevPage" 
          :disabled="currentPage === 1"
          class="px-4 py-2 text-sm font-medium text-theme-text bg-theme-bg rounded-lg hover:bg-theme-border disabled:opacity-50 disabled:cursor-not-allowed dark:bg-dark-card dark:text-dark-text dark:hover:bg-dark-border transition-colors shadow-sm"
        >
          上一页
        </button>
        
        <span class="text-sm text-theme-text dark:text-dark-text">
          第 {{ currentPage }} / {{ totalPages }} 页
        </span>

        <button 
          @click="nextPage" 
          :disabled="currentPage === totalPages"
          class="px-4 py-2 text-sm font-medium text-theme-text bg-theme-bg rounded-lg hover:bg-theme-border disabled:opacity-50 disabled:cursor-not-allowed dark:bg-dark-card dark:text-dark-text dark:hover:bg-dark-border transition-colors shadow-sm"
        >
          下一页
        </button>
      </div>
    </div>
    
  </div>
</template>
