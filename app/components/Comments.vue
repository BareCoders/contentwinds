<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'

onMounted(() => {
  // 创建 giscus 的 script 标签
  const script = document.createElement('script')
  script.src = 'https://giscus.app/client.js'
  
  // 写入你提供的配置项
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

  // 检查当前是亮色还是暗色模式，自动匹配主题（可选）
  const isDark = document.documentElement.classList.contains('dark')
  // 你的原配置里是 light_tritanopia，如果想自动适配，可以改成：
  // script.setAttribute('data-theme', isDark ? 'dark' : 'light')
  script.setAttribute('data-theme', 'light_tritanopia') 

  // 把 script 挂载到一个特定的 div 容器上
  const container = document.getElementById('giscus-container')
  if (container) {
    container.appendChild(script)
  }
})

onUnmounted(() => {
  // 页面离开时清理，防止内存泄漏
  const container = document.getElementById('giscus-container')
  if (container) {
    container.innerHTML = ''
  }
})
</script>

<template>
  <div class="mt-12 pt-8 border-t border-gray-200 dark:border-gray-700">
    <h3 class="text-xl font-bold mb-4">发表评论</h3>
    <div id="giscus-container"></div>
  </div>
</template>
