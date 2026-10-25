<script setup lang="ts">
import { onMounted, onUnmounted, watch } from 'vue'

let observer: MutationObserver | null = null

// 初始化 Giscus
const initGiscus = () => {
  const container = document.getElementById('giscus-container')
  if (!container) return

  // 清空旧内容
  container.innerHTML = ''

  // 判断当前主题 (如果你的暗色模式会在 html 标签加 class="dark")
  const isDark = document.documentElement.classList.contains('dark')
  
  // 动态匹配主题：亮色用 github-light，暗色用 github-dark
  const theme = isDark ? 'github-dark' : 'github-light'

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
  
  // 设置主题
  script.setAttribute('data-theme', theme)

  container.appendChild(script)
}

onMounted(() => {
  // 首次加载
  initGiscus()

  // 监听 HTML 标签的 class 变化（监听用户点击切换暗黑模式）
  observer = new MutationObserver(() => {
    initGiscus()
  })
  observer.observe(document.documentElement, { attributes: true, attributeFilter: ['class'] })
})

onUnmounted(() => {
  // 页面销毁时移除监听，清理容器
  if (observer) observer.disconnect()
  const container = document.getElementById('giscus-container')
  if (container) container.innerHTML = ''
})
</script>

<template>
  <div class="mt-12 pt-8 border-t border-gray-200 dark:border-gray-700">
    <h3 class="text-xl font-bold mb-4">发表评论</h3>
    <div id="giscus-container"></div>
  </div>
</template>
