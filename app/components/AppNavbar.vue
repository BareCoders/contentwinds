<script setup lang="ts">
const { data: navigation } = await useAsyncData('navigation', () =>
  queryCollectionNavigation('content'),
);

const appConfig = useAppConfig();
</script>

<template>
  <!-- 
    终极单文件解决方案：
    1. 外层加上 pt-[80px]（如果你觉得不够，可以改成 pt-[90px] 或 pt-[100px]）。
    2. 真正的导航栏用 absolute 贴在屏幕最顶部。
    这样不用去改 app.vue，也能把文章内容强行往下挤！
  -->
  <div class="relative w-full pt-[80px]">
    
    <!-- 导航栏本体：绝对固定 + 最高层级 + 纯白背景 -->
    <div class="absolute top-0 left-0 right-0 z-[999] w-full bg-white dark:bg-[#191a1c] border-b border-gray-200 dark:border-gray-800 shadow-sm">
      
      <div class="flex justify-between items-center px-4 py-4 mx-auto sm:px-8 w-full">
        
        <!-- 左侧导航 -->
        <div class="text-gray-700 dark:text-gray-200 flex gap-4 sm:gap-6 overflow-x-auto whitespace-nowrap">
          <NuxtLink
            v-for="link in navigation"
            :key="link.path" 
            :to="link.path"
            class="hover:text-[#46677e] dark:hover:text-[#9fbcd0] transition-colors"
            active-class="font-bold text-[#46677e] dark:text-[#9fbcd0]"
          >{{ link.title }}</NuxtLink>
        </div>
        
        <!-- 右侧社交图标与暗黑模式 -->
        <div class="text-gray-500 flex items-center gap-3 sm:gap-4 ml-4">
          <a
            v-if="appConfig.socials?.twitter || appConfig.socials?.x"
            class="dark:text-gray-100 hover:text-gray-700 dark:hover:text-gray-300"
            :href="`https://x.com/${appConfig.socials?.twitter || appConfig.socials?.x}`"
            title="X"
          >
            <Icon class="size-5" name="i-simple-icons-x" />
          </a>
          <a
            v-if="appConfig.socials?.github"
            class="dark:text-gray-100 hover:text-gray-700 dark:hover:text-gray-300"
            :href="`https://github.com/${appConfig.socials?.github}`"
            title="GitHub"
          >
            <Icon class="size-5" name="i-simple-icons-github" />
          </a>
          <a
            v-if="appConfig.socials?.mastodon"
            class="dark:text-gray-100 hover:text-gray-700 dark:hover:text-gray-300"
            :href="`https://elk.zone/${appConfig.socials?.mastodon}`" title="Mastodon"
          >
            <Icon class="size-5" name="i-simple-icons-mastodon" />
          </a>
          <a
            v-if="appConfig.socials?.bluesky"
            class="dark:text-gray-100 hover:text-gray-700 dark:hover:text-gray-300"
            :href="`https://bsky.app/profile/${appConfig.socials?.bluesky}`" title="Bluesky"
          >
            <Icon class="size-5" name="i-simple-icons-bluesky" />
          </a>
          <ColorModeSwitch class="dark:text-gray-100 hover:text-gray-700 dark:hover:text-gray-300" />
        </div>
        
      </div>
    </div>
    
  </div>
</template>
