<script setup lang="ts">
import { ref, onMounted, onUnmounted, watch, nextTick } from 'vue'
import { useRoute } from 'vue-router'
import { init } from '@waline/client'
import '@waline/client/style'

const route = useRoute()

// =========================================================================
// 👇 一键开关区：想关掉谁，就把谁改成 false 即可！
// =========================================================================
const enableWaline = true;  // 👈 设为 false 则关闭 Waline 快捷评论
const enableGiscus = true;  // 👈 设为 false 则关闭 Giscus (GitHub) 评论
// =========================================================================

// 根据开关状态，决定默认显示哪个
let defaultTab: 'waline' | 'giscus' = 'waline'
if (enableWaline) defaultTab = 'waline'
else if (enableGiscus) defaultTab = 'giscus'

const activeTab = ref<'waline' | 'giscus'>(defaultTab)

let walineInstance: any = null
let giscusObserver: MutationObserver | null = null

// --- Waline 初始化 ---
const initWaline = () => {
  if (!enableWaline) return
  const el = document.getElementById('waline-container')
  if (!el) return
  if (walineInstance) walineInstance.destroy()

  walineInstance = init({
    el: '#waline-container',
    serverURL: 'https://gb.ooo', // 你的 Waline 地址
    path: route.path,
    lang: 'zh-CN',
    dark: '.dark',
    reaction: true,
  })
}

// --- Giscus 初始化 ---
const initGiscus = () => {
  if (!enableGiscus) return
  const container = document.getElementById('giscus-container')
  if (!container) return

  container.innerHTML = ''
  const isDark = document.documentElement.classList.contains('dark')
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

  if (giscusObserver) giscusObserver.disconnect()
  giscusObserver = new MutationObserver(() => {
    initGiscus()
  })
  giscusObserver.observe(document.documentElement, { attributes: true, attributeFilter: ['class'] })
}

// 监听切换
const switchTab = async (tab: 'waline' | 'giscus') => {
  activeTab.value = tab
  await nextTick()
  if (tab === 'waline') initWaline()
  else initGiscus()
}

onMounted(() => {
  if (activeTab.value === 'waline') initWaline()
  else initGiscus()
})

watch(() => route.path, () => {
  if (enableWaline && enableGiscus) activeTab.value = 'waline'
  nextTick(() => {
    if (activeTab.value === 'waline') initWaline()
    else initGiscus()
  })
})

onUnmounted(() => {
  if (walineInstance) walineInstance.destroy()
  if (giscusObserver) giscusObserver.disconnect()
})
</script>

<template>
  <!-- 如果两个都关了，则不渲染任何东西 -->
  <div v-if="enableWaline || enableGiscus" class="mt-12 pt-8 border-t border-dashed border-theme-border dark:border-dark-border">
    
    <div class="flex flex-col sm:flex-row sm:items-center justify-between mb-6 gap-4">
      <h3 class="text-xl font-bold text-theme-title dark:text-dark-title">发表评论</h3>
      
      <!-- 👇 只有在两个都开启的情况下，才显示切换按钮 -->
      <div v-if="enableWaline && enableGiscus" class="flex items-center bg-[#f6f8fa] dark:bg-[#161b22] p-1 rounded-lg border border-theme-border dark:border-dark-border w-fit">
        <button 
          @click="switchTab('waline')"
          :class="['px-4 py-1.5 text-sm font-medium rounded-md transition-all', activeTab === 'waline' ? 'bg-white dark:bg-[#30363d] shadow-sm text-[#0969da] dark:text-[#58a6ff]' : 'text-theme-text hover:text-theme-title dark:hover:text-white']"
        >
          Waline 快捷评论
        </button>
        <button 
          @click="switchTab('giscus')"
          :class="['px-4 py-1.5 text-sm font-medium rounded-md transition-all', activeTab === 'giscus' ? 'bg-white dark:bg-[#30363d] shadow-sm text-[#0969da] dark:text-[#58a6ff]' : 'text-theme-text hover:text-theme-title dark:hover:text-white']"
        >
          GitHub 登录评论
        </button>
      </div>
    </div>

    <div class="min-h-[200px]">
      <div v-if="enableWaline" v-show="activeTab === 'waline'" id="waline-container" class="waline-wrapper"></div>
      <div v-if="enableGiscus" v-show="activeTab === 'giscus'" id="giscus-container" class="relative z-10"></div>
    </div>

  </div>
</template>

<style>
.waline-wrapper {
  --waline-font-size: 14px;
  --waline-theme-color: #0969da;
  --waline-active-color: #1d4ed8;
}
.dark .waline-wrapper {
  --waline-theme-color: #58a6ff;
  --waline-active-color: #79c0ff;
  --waline-bg-color: transparent;
}
</style>
