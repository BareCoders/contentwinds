<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const isVisible = ref(false)

const handleScroll = () => {
  isVisible.value = window.scrollY > 300
}

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
  <Transition
    enter-active-class="transition duration-300 ease-out"
    enter-from-class="translate-y-4 opacity-0"
    enter-to-class="translate-y-0 opacity-100"
    leave-active-class="transition duration-200 ease-in"
    leave-from-class="translate-y-0 opacity-100"
    leave-to-class="translate-y-4 opacity-0"
  >
    <button
      v-show="isVisible"
      @click="scrollToTop"
      aria-label="回到顶部"
      class="fixed bottom-6 right-6 z-50 p-3 rounded-full flex items-center justify-center transition-all duration-300 hover:-translate-y-1 hover:shadow-md
             bg-white/80 dark:bg-[#161b22]/80 backdrop-blur-md
             border border-[#d0d7de] dark:border-[#30363d] shadow-sm"
    >
      <!-- 
        箭头图标：使用 SVG 原生渐变 
        stroke-width="3" 实现加粗
      -->
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="3" class="w-5 h-5">
        <defs>
          <!-- 定义一个渐变色：从紫蓝到亮蓝 -->
          <linearGradient id="arrow-gradient" x1="0%" y1="100%" x2="100%" y2="0%">
            <stop offset="0%" stop-color="#8b5cf6" /> <!-- 紫色 -->
            <stop offset="100%" stop-color="#3b82f6" /> <!-- 亮蓝色 -->
          </linearGradient>
        </defs>
        <!-- 应用渐变色 -->
        <path stroke-linecap="round" stroke-linejoin="round" stroke="url(#arrow-gradient)" d="M4.5 15.75l7.5-7.5 7.5 7.5" />
      </svg>
    </button>
  </Transition>
</template>
