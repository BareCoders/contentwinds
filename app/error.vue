<script setup lang="ts">
import type { NuxtError } from '#app'

const props = defineProps({
  error: Object as () => NuxtError | null
})

const handleError = () => clearError({ redirect: '/' })
</script>

<template>
  <div class="error-page min-h-screen flex flex-col text-[#334155] dark:text-[#c9d1d9] transition-colors duration-300">
    
    <!-- 极简手写导航：防止因为调用外部组件导致环境报错白屏 -->
    <header class="w-full flex justify-between items-center px-6 py-6 sm:px-12 relative z-10">
      <a href="/" class="text-xl font-bold tracking-wider text-[#334155] dark:text-white hover:opacity-70 transition-opacity">我的博客</a>
      <a href="/" class="text-sm font-medium text-[#334155] dark:text-white hover:opacity-70 transition-opacity">回主页</a>
    </header>

    <!-- 主体内容 -->
    <main class="flex-grow flex items-center justify-center px-4 relative z-10">
      <div class="text-center flex flex-col items-center">
        
        <!-- 超大号 404 背景字 -->
        <h1 class="text-8xl sm:text-9xl font-black mb-6 text-[#334155]/20 dark:text-white/10 select-none">
          {{ props.error?.statusCode || 404 }}
        </h1>
        
        <h2 class="text-2xl sm:text-3xl font-bold mb-4 text-[#1e293b] dark:text-white">
          哎呀，页面走丢了
        </h2>
        
        <p class="opacity-70 mb-8 max-w-md mx-auto text-sm sm:text-base">
          你访问的页面可能被外星人吸走，或者链接输入有误。<br>
          检查一下网址，或者回到安全区吧。
        </p>
        
        <button 
          @click="handleError"
          class="px-8 py-3 text-sm font-medium text-white bg-[#0969da] rounded-lg hover:bg-[#1d4ed8] dark:bg-[#58a6ff] dark:text-[#0d1117] dark:hover:bg-[#79c0ff] transition-all hover:-translate-y-0.5 shadow-md"
        >
          返回首页
        </button>
      </div>
    </main>

    <!-- 极简手写页脚 -->
    <footer class="w-full text-center py-6 text-xs text-[#64748b] dark:text-[#8b949e] relative z-10">
      © 2026 我的博客. All rights reserved.
    </footer>
  </div>
</template>

<style>
/* 
  🌟 100% 还原 Nuxt 官方 404 的背景渐变（多层径向渐变光晕）
  左下薄荷绿、右下冰蓝，融合于左上和右上的纯白。
*/
.error-page {
  background-color: #fafafa;
  background-image: 
    radial-gradient(at 0% 0%, rgba(255,255,255,1) 0px, transparent 50%),
    radial-gradient(at 100% 0%, rgba(255,255,255,1) 0px, transparent 50%),
    radial-gradient(at 100% 100%, rgba(0, 195, 255, 0.15) 0px, transparent 50%),
    radial-gradient(at 0% 100%, rgba(0, 220, 130, 0.15) 0px, transparent 50%);
  background-attachment: fixed;
}

/* 深色模式的渐变 */
html.dark .error-page {
  background-color: #0d1117;
  background-image: 
    radial-gradient(at 0% 100%, rgba(0, 220, 130, 0.05) 0px, transparent 50%),
    radial-gradient(at 100% 100%, rgba(0, 195, 255, 0.05) 0px, transparent 50%);
}
</style>
