<script setup lang="ts">
const { data: navigation } = await useAsyncData('navigation', () =>
  queryCollectionNavigation('content'),
);

const appConfig = useAppConfig();
</script>

<template>
  <!-- 外层：fixed 固定在顶部，z-[999] 防止被遮挡 -->
  <div class="fixed top-0 left-0 right-0 z-[999] w-full bg-white dark:bg-[#191a1c] border-b border-gray-200 dark:border-gray-800 shadow-md">
    
    <div class="flex justify-between items-center px-4 py-4 mx-auto sm:px-8 w-full">
      
      <!-- 左侧导航：强制改为 text-gray-900（近乎纯黑），暗黑模式为白色 -->
      <div class="text-gray-900 dark:text-white flex gap-4 sm:gap-6 overflow-x-auto whitespace-nowrap font-medium">
        <NuxtLink
          v-for="link in navigation"
          :key="link.path" 
          :to="link.path"
          class="hover:text-[#46677e] dark:hover:text-[#9fbcd0] transition-colors"
          active-class="font-bold text-[#46677e] dark:text-[#9fbcd0]"
        >{{ link.title }}</NuxtLink>
      </div>
      
      <!-- 右侧社交图标与暗黑模式 -->
      <div class="text-gray-600 dark:text-gray-300 flex items-center gap-3 sm:gap-4 ml-4">
        <a
          v-if="appConfig.socials?.twitter || appConfig.socials?.x"
          class="hover:text-black dark:hover:text-white"
          :href="`https://x.com/${appConfig.socials?.twitter || appConfig.socials?.x}`"
          title="X"
        >
          <Icon class="size-5" name="i-simple-icons-x" />
        </a>
        <a
          v-if="appConfig.socials?.github"
          class="hover:text-black dark:hover:text-white"
          :href="`https://github.com/${appConfig.socials?.github}`"
          title="GitHub"
        >
          <Icon class="size-5" name="i-simple-icons-github" />
        </a>
        <a
          v-if="appConfig.socials?.mastodon"
          class="hover:text-black dark:hover:text-white"
          :href="`https://elk.zone/${appConfig.socials?.mastodon}`" title="Mastodon"
        >
          <Icon class="size-5" name="i-simple-icons-mastodon" />
        </a>
        <a
          v-if="appConfig.socials?.bluesky"
          class="hover:text-black dark:hover:text-white"
          :href="`https://bsky.app/profile/${appConfig.socials?.bluesky}`" title="Bluesky"
        >
          <Icon class="size-5" name="i-simple-icons-bluesky" />
        </a>
        <ColorModeSwitch class="hover:text-black dark:hover:text-white" />
      </div>
      
    </div>
  </div>
</template>
