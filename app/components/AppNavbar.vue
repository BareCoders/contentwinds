<script setup lang="ts">
const { data: navigation } = await useAsyncData('navigation', () =>
  queryCollectionNavigation('content'),
);

const appConfig = useAppConfig();
</script>

<template>
  <!-- 外层：绝对固定，z-50最高层级，毛玻璃背景 -->
  <div class="fixed top-0 left-0 right-0 z-50 w-full bg-white/80 dark:bg-[#191a1c]/80 backdrop-blur-md border-b border-gray-100 dark:border-gray-800 transition-all">
    
    <!-- 内层：去掉 max-w-2xl，改为 w-full 和你 80% 的排版对齐 -->
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
</template>
