<script setup lang="ts">
import type { NuxtError } from '#app'

const props = defineProps({
  error: Object as () => NuxtError | null
})

const handleError = () => clearError({ redirect: '/' })
</script>

<template>
  <!-- 
    1. 使用 error-bg 类实现深度渐变。
    2. 直接调用原装的 <AppNavbar /> 和 <AppFooter />。
  -->
  <div class="error-bg min-h-screen flex flex-col text-[#334155] dark:text-[#c9d1d9] transition-colors duration-300">
    
    <!-- 完整导航栏 -->
    <AppNavbar />

    <!-- 主体内容：pt-32 留出足够的空间，防止被固定的导航栏遮挡 -->
    <main class="flex-grow flex items-center justify-center px-4 pt-32 pb-10 relative z-10">
      <div class="text-center flex flex-col items-center">
        
        <!-- 超大号 404 -->
        <h1 class="text-8xl sm:text-9xl font-black mb-6 text-[#334155] dark:text-white/20 select-none">
          {{ props.error?.statusCode || 404 }}
        </h1>
        
        <h2 class="text-2xl sm:text-3xl font-bold mb-4 text-[#1e293b] dark:text-white">
          哎呀，页面走丢了
        </h2>
        
        <p class="opacity-80 mb-8 max-w-md mx-auto text-sm sm:text-base">
          你访问的页面可能被外星人吸走，或者链接输入有误。<br>
          检查一下网址，或者回到安全区吧。
        </p>
        
        <button 
          @click="handleError"
          class="px-8 py-3 text-sm font-medium text-white bg-[#0969da] rounded-lg hover:bg-[#1d4ed8] dark:bg-[#58a6ff] dark:text-[#0d1117] dark:hover:bg-[#79c0ff] transition-all hover:-translate-y-0.5 shadow-lg"
        >
          返回首页
        </button>
      </div>
    </main>

    <!-- 完整页脚 -->
    <AppFooter />
  </div>
</template>

<style>
/* 
  🌟 深度还原图 2 的渐变光晕背景
*/
.error-bg {
  background-color: #e2e8f0;
  background-image: 
    radial-gradient(at 0% 0%, rgba(255, 255, 255, 1) 0%, transparent 60%),
    radial-gradient(at 100% 100%, rgba(153, 246, 228, 0.8) 0%, transparent 70%), /* 薄荷绿 */
    radial-gradient(at 100% 0%, rgba(186, 230, 253, 0.8) 0%, transparent 70%),   /* 浅蓝 */
    radial-gradient(at 0% 100%, rgba(255, 255, 255, 0.8) 0%, transparent 60%);
  background-attachment: fixed;
}

/* 深色模式的渐变 */
html.dark .error-bg {
  background-color: #0d1117;
  background-image: 
    radial-gradient(at 100% 100%, rgba(20, 184, 166, 0.15) 0%, transparent 60%),
    radial-gradient(at 100% 0%, rgba(56, 189, 248, 0.1) 0%, transparent 60%);
}
</style>
