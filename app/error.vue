<script setup lang="ts">
import type { NuxtError } from '#app'

const props = defineProps({
  error: Object as () => NuxtError
})

const handleError = () => clearError({ redirect: '/' })
</script>

<template>
  <!-- 
    1. 使用了 bg-gradient-to-br 实现从浅蓝到薄荷绿的渐变。
    2. 暗黑模式下自动回退到深色背景。
  -->
  <div class="min-h-screen flex flex-col relative overflow-hidden bg-gradient-to-br from-[#e6f0fa] via-[#f4f9f4] to-[#d0e4dc] dark:bg-none dark:bg-dark-bg text-theme-text dark:text-dark-text transition-colors">
    
    <!-- 导航栏 -->
    <AppNavbar />
    
    <!-- 主体内容：居中显示 -->
    <main class="flex-grow flex items-center justify-center relative z-10 px-4 pt-24 pb-10">
      
      <!-- 为了让渐变背景透出来，这里不用之前的白卡片，改成完全透明，让字直接漂浮在渐变上 -->
      <div class="text-center flex flex-col items-center justify-center py-16 px-6 sm:py-20">
        
        <!-- 404 大字（用深色或渐变色，这里用带一点点透明度的黑） -->
        <h1 class="text-8xl sm:text-9xl font-black mb-6 text-gray-800/20 dark:text-white/10 select-none">
          {{ error?.statusCode || 404 }}
        </h1>
        
        <h2 class="text-2xl font-bold mb-4 text-[#334155] dark:text-dark-title">
          哎呀，页面走丢了
        </h2>
        
        <p class="text-[#64748b] dark:text-dark-text mb-8 max-w-md mx-auto">
          你访问的页面可能被外星人吸走，或者链接输入有误。<br>检查一下网址，或者回到安全区吧。
        </p>
        
        <button 
          @click="handleError"
          class="px-6 py-3 text-sm font-medium text-white bg-[#0969da] rounded-lg hover:bg-[#1d4ed8] dark:bg-dark-link dark:text-dark-bg dark:hover:bg-[#79c0ff] transition-all hover:-translate-y-0.5 shadow-md"
        >
          返回首页
        </button>
      </div>
    </main>
    
    <!-- 页脚 -->
    <AppFooter />
  </div>
</template>
