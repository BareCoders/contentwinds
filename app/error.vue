<script setup lang="ts">
import type { NuxtError } from '#app'

const props = defineProps({
  error: Object as () => NuxtError
})

const handleError = () => clearError({ redirect: '/' })
</script>

<template>
  <div class="min-h-screen flex flex-col pt-20 pb-10 bg-theme-bg text-theme-text dark:bg-dark-bg dark:text-dark-text transition-colors">
    <!-- 简单放个导航，也可以直接用 AppNavbar -->
    <AppNavbar />
    
    <main class="flex-grow flex items-center justify-center">
      <div class="card-glass text-center flex flex-col items-center justify-center py-20">
        <h1 class="text-8xl font-black text-[#d0d7de] dark:text-[#30363d] mb-4">
          {{ error?.statusCode || 404 }}
        </h1>
        
        <h2 class="text-2xl font-bold text-theme-title dark:text-dark-title mb-4">
          哎呀，页面走丢了
        </h2>
        
        <p class="text-theme-text dark:text-dark-text mb-8 max-w-md mx-auto">
          你访问的页面可能被外星人吸走，或者链接输入有误。检查一下网址，或者回到安全区吧。
        </p>
        
        <button 
          @click="handleError"
          class="px-6 py-3 text-sm font-medium text-white bg-[#0969da] rounded-lg hover:bg-[#1d4ed8] dark:bg-dark-link dark:text-dark-bg dark:hover:bg-[#79c0ff] transition-colors shadow-sm"
        >
          返回首页
        </button>
      </div>
    </main>
    
    <AppFooter />
  </div>
</template>
