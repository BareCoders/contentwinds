<script setup lang="ts">
const { cover } = useAppConfig()

useSeoMeta({
  ogImage: cover,
  twitterCard: 'summary_large_image',
})
</script>

<template>
  <!-- 使用 flex 布局，让页脚始终在页面最底部 -->
  <div class="min-h-screen flex flex-col sm:pt-6 sm:pb-10">
    <Html lang="en" />
    <NuxtLoadingIndicator />
    
    <!-- 顶部导航 -->
    <AppNavbar />
    
    <!-- 页面主体内容（flex-grow 让它撑满剩余空间） -->
    <main class="flex-grow">
      <!-- 新增了 w-[80%] mx-auto，让页面占屏幕 80% 宽并居中 -->
      <div class="w-[80%] mx-auto">
        <NuxtPage />
      </div>
    </main>
    
    <!-- 底部页脚 -->
    <AppFooter />
  </div>
</template>
