<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'

let observer: MutationObserver | null = null

const initGiscus = () => {
  const container = document.getElementById('giscus-container')
  if (!container) return

  container.innerHTML = ''
  const isDark = document.documentElement.classList.contains('dark')
  
  // 浅色模式使用透明背景 transparent_light，深色模式使用 dark_dimmed
  const theme = isDark ? 'dark_dimmed' : 'transparent_light'

  const script = document.createElement('script')
  script.src = 'https://giscus.app/client.js'
  
  script.setAttribute('data-repo', 'WorkSpacesPro/guestbook')
  script.setAttribute('data-repo-id', 'R_kgDOOEORuw')
  script.setAttribute('data-category', 'General')
  script.setAttribute('data-category-id', 'DIC_kwDOOEORu84Cp384')
  script.setAttribute('data-mapping', 'pathname')
  script.setAttribute('data-strict', '0')
  script.setAttribute('data-reactions-enabled', '1')
  script.setAttribute('data-emit-metadata', '0')
  script.setAttribute('data-input-position', 'bottom')
  script.setAttribute('data-lang', 'zh-CN')
  script.setAttribute('crossorigin', 'anonymous')
  script.async = true
  script.setAttribute('data-theme', theme)

  container.appendChild(script)
}

onMounted(() => {
  initGiscus()
  // 监听暗黑模式切换
  observer = new MutationObserver(() => {
    initGiscus()
  })
  observer.observe(document.documentElement, { attributes: true, attributeFilter: ['class'] })
})

onUnmounted(() => {
  if (observer) observer.disconnect()
  const container = document.getElementById('giscus-container')
  if (container) container.innerHTML = ''
})
</script>

<template>
  <!-- 调整了边框颜色，使用微透明边框与整体卡片融合 -->
  
  <div class="mt-12 pt-8 border-t border-dashed border-gray-300/60 dark:border-gray-700/60">
    
    <h3 class="text-xl font-bold mb-4 text-[#1e293b] dark:text-white">发表评论</h3>
    <div id="giscus-container" class="relative z-10"></div>
  </div>
</template>
