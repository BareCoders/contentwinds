<script setup lang="ts">
// 1. 获取 blog 下的所有的文章
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

// ================= 分页逻辑 数字 首页显示文章数量=================
const currentPage = ref(1)
const pageSize = 15

const paginatedPosts = computed(() => {
  const start = (currentPage.value - 1) * pageSize
  const end = start + pageSize
  return sortedPosts.value.slice(start, end)
})

const totalPages = computed(() => {
  return Math.ceil(sortedPosts.value.length / pageSize)
})

const nextPage = () => {
  if (currentPage.value < totalPages.value) currentPage.value++
}
const prevPage = () => {
  if (currentPage.value > 1) currentPage.value--
}
// ============================================
</script>

<template>
  <!-- 👇 核心修改：加上 overflow-hidden，保护外层卡片的圆角不被全宽斑马线破坏 -->
  <div class="card-glass flex flex-col overflow-hidden">
    
    <!-- 标题虚线：保持简短克制 -->
    <div class="w-fit pb-2 mb-6 ml-4 sm:ml-8 border-b divider-dashed">
      <h1 class="text-3xl font-bold text-theme-title dark:text-dark-title">博客文章</h1>
    </div>

    <div v-if="!paginatedPosts || paginatedPosts.length === 0" class="text-theme-text dark:text-dark-text px-4 sm:px-8">
      暂时还没有文章哦~
    </div>

    <div v-else class="flex-grow flex flex-col">
      
      <!-- 👇 恢复全宽偏移：-mx-4 sm:-mx-8 让斑马线撑满整个卡片 -->
      <div class="space-y-0 -mx-4 sm:-mx-8">
        <NuxtLink 
          v-for="(post, index) in paginatedPosts" 
          :key="post.path" 
          :to="post.path"
          class="group flex items-center justify-between py-4 px-4 sm:px-8 border-b divider-dashed last:border-0 transition-colors hover:!bg-[#eaeef2] dark:hover:!bg-white/10"
          :class="index % 2 === 0 ? 'bg-[#f6f8fa] dark:bg-[#21262d]' : 'bg-transparent'"
        >
          <!-- 👇 truncate 防止标题太长换行，pr-4 留出和日期的间距 -->
          <span class="text-base font-medium text-[#0969da] dark:text-dark-link group-hover:underline decoration-1 underline-offset-4 truncate pr-4">
            {{ post.title }}
          </span>
          
          <!-- 👇 shrink-0 保证日期永远不被挤压 -->
          <span class="text-xs text-theme-text/70 dark:text-dark-text/70 whitespace-nowrap shrink-0">
            {{ post.displayDate }}
          </span>
        </NuxtLink>
      </div>

      <!-- 翻页按钮区域 -->
      <div class="flex items-center justify-between mt-auto pt-6 mx-4 sm:mx-8 border-t divider-dashed">
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
