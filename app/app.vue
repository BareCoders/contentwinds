<script setup lang="ts">
import { onMounted, watch } from 'vue'
import { useRoute } from 'vue-router'

const { cover } = useAppConfig()
const route = useRoute()

useSeoMeta({
  ogImage: cover,
  twitterCard: 'summary_large_image',
})

// 自动给所有外部链接加上新窗口打开
const handleExternalLinks = () => {
  setTimeout(() => {
    const links = document.querySelectorAll('a')
    links.forEach((link) => {
      const href = link.getAttribute('href')
      if (href && href.startsWith('http') && !href.includes(window.location.hostname)) {
        link.setAttribute('target', '_blank')
        link.setAttribute('rel', 'noopener noreferrer')
      }
    })
  }, 100)
}

onMounted(() => {
  handleExternalLinks()
})

watch(() => route.path, () => {
  handleExternalLinks()
})
</script>

<template>
  <!-- 
    核心修复：把 bg-[#dbeafe] 换成了 bg-theme-bg，把文字颜色也换成了 text-theme-text。
    这样它就会乖乖去读取 main.css 里配置的 GitHub 灰白了。
  -->
  <div class="min-h-screen flex flex-col pt-20 pb-10 bg-theme-bg dark:bg-dark-bg text-theme-text dark:text-dark-text transition-colors">
    <Html lang="en" />
    <NuxtLoadingIndicator />
    
    <AppNavbar />
    
    <main class="flex-grow">
      <!-- 统一限制为 80% -->
      <div class="w-[80%] mx-auto">
        <NuxtPage />
      </div>
    </main>
    
    <!-- footer -->
    <AppFooter />
    <!-- 回到顶部 -->
    <BackToTop />
  </div>
</template>

<style>
/* 强制覆盖：只要页面里还有 max-w-3xl，统统让它变成 100% 宽 */
main .max-w-3xl {
  max-width: 100% !important;
}
</style>
