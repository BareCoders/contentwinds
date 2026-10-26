<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const isVisible = ref(false)

// 监听滚动事件，滑动超过 300px 时显示按钮
const handleScroll = () => {
  isVisible.value = window.scrollY > 300
}

// 点击后平滑滚动到顶部
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
  <!-- 
    使用 Transition 实现淡入淡出效果
    毛玻璃背景 + 向上箭头
  -->
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
      class="fixed bottom-8 right-8 z-50 p-3 rounded-full bg-white/40 dark:bg-[#191a1c]/40 backdrop-blur-md ring-1 ring-white/50 dark:ring-gray-700 shadow-sm text-gray-700 dark:text-gray-200 hover:bg-white/60 dark:hover:bg-[#191a1c]/60 hover:-translate-y-1 transition-all flex items-center justify-center"
      aria-label="回到顶部"
    >
      <!-- 向上箭头图标 -->
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-5 h-5">
        <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 15.75l7.5-7.5 7.5 7.5" />
      </svg>
    </button>
  </Transition>
</template>
