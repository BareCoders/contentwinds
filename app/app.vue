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
  // 延迟一小会儿，确保页面内容（包括文章）已经完全渲染
  setTimeout(() => {
    // 找到页面里所有的 <a> 标签
    const links = document.querySelectorAll('a')
    links.forEach((link) => {
      const href = link.getAttribute('href')
      // 如果有 href，且是以 http 开头，且不包含自己的域名，那就是外部链接
      if (href && href.startsWith('http') && !href.includes(window.location.hostname)) {
        link.setAttribute('target', '_blank')
        link.setAttribute('rel', 'noopener noreferrer') // 安全属性，防止钓鱼
      }
    })
  }, 100)
}

// 首次加载执行
onMounted(() => {
  handleExternalLinks()
})

// 监听路由变化，每次切换页面都重新扫描一遍（因为 Vue 是单页应用）
watch(() => route.path, () => {
  handleExternalLinks()
})
</script>

<template>
  <div class="min-h-screen flex flex-col pt-20 pb-10 bg-[#dbeafe] dark:bg-[#191a1c] text-[#334155] dark:text-gray-200 transition-colors">
    <Html lang="en" />
    <NuxtLoadingIndicator />
    
    <AppNavbar />
    
    <main class="flex-grow">
      <!-- 统一限制为 80% -->
      <div class="w-[80%] mx-auto">
        <NuxtPage />
      </div>
    </main>
    
    <AppFooter />
  </div>
</template>

<style>
/* 强制覆盖：只要页面里还有 max-w-3xl，统统让它变成 100% 宽 */
main .max-w-3xl {
  max-width: 100% !important;
}
</style>
