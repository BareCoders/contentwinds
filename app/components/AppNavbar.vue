<script setup lang="ts">
const { data: navigation } = await useAsyncData('navigation', () =>
  queryCollectionNavigation('content'),
);

const appConfig = useAppConfig();
</script>

<template>
  <!-- 
    最外层直接调用 main.css 中定义的 nav-glass
    包含了：固定吸顶、半透明磨砂背景、底部边框线、阴影
  -->
  <div class="nav-glass">
    
    <!-- 内层控制布局和间距 -->
    <div class="flex justify-between items-center px-4 py-4 mx-auto sm:px-8 w-full">
      
      <!-- 左侧导航菜单 -->
      <div class="text-theme-text dark:text-dark-text flex gap-4 sm:gap-6 overflow-x-auto whitespace-nowrap font-medium">
        <NuxtLink
          v-for="link in navigation"
          :key="link.path" 
          :to="link.path"
          class="hover:text-[#2563eb] dark:hover:text-dark-link transition-colors"
          active-class="font-bold text-[#2563eb] dark:text-dark-link"
        >{{ link.title }}</NuxtLink>
      </div>
      
      <!-- 右侧社交图标与暗黑模式开关 -->
      <div class="text-theme-text dark:text-dark-text flex items-center gap-3 sm:gap-4 ml-4">
        <!-- X (Twitter) -->
        <a
          v-if="appConfig.socials?.twitter || appConfig.socials?.x"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://x.com/${appConfig.socials?.twitter || appConfig.socials?.x}`"
          target="_blank"
          rel="noopener noreferrer"
          title="X"
        >
          <Icon class="size-5" name="i-simple-icons-x" />
        </a>
        
        <!-- GitHub -->
        <a
          v-if="appConfig.socials?.github"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://github.com/${appConfig.socials?.github}`"
          target="_blank"
          rel="noopener noreferrer"
          title="GitHub"
        >
          <Icon class="size-5" name="i-simple-icons-github" />
        </a>
        
        <!-- Mastodon -->
        <a
          v-if="appConfig.socials?.mastodon"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://elk.zone/${appConfig.socials?.mastodon}`" 
          target="_blank"
          rel="noopener noreferrer"
          title="Mastodon"
        >
          <Icon class="size-5" name="i-simple-icons-mastodon" />
        </a>
        
        <!-- Bluesky -->
        <a
          v-if="appConfig.socials?.bluesky"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://bsky.app/profile/${appConfig.socials?.bluesky}`" 
          target="_blank"
          rel="noopener noreferrer"
          title="Bluesky"
        >
          <Icon class="size-5" name="i-simple-icons-bluesky" />
        </a>
        
        <!-- 深浅模式切换 -->
        <ColorModeSwitch class="hover:text-black dark:hover:text-white transition-colors" />
      </div>
      
    </div>
  </div>
</template>
