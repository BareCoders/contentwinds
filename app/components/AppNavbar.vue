<script setup lang="ts">
const { data: navigation } = await useAsyncData('navigation', () =>
  queryCollectionNavigation('content'),
);

const appConfig = useAppConfig();
</script>

<template>
  <!-- 只替换了最外层这一行，改为调用全局的 nav-glass 类 -->
  <div class="nav-glass">
    
    <div class="flex justify-between items-center px-4 py-4 mx-auto sm:px-8 w-full">
      
      <!-- 左侧导航 -->
      <div class="text-theme-text dark:text-dark-text flex gap-4 sm:gap-6 overflow-x-auto whitespace-nowrap font-medium">
        <NuxtLink
          v-for="link in navigation"
          :key="link.path" 
          :to="link.path"
          class="hover:text-[#2563eb] dark:hover:text-dark-link transition-colors"
          active-class="font-bold text-[#2563eb] dark:text-dark-link"
        >{{ link.title }}</NuxtLink>
      </div>
      
      <!-- 右侧社交图标与暗黑模式 -->
      <div class="text-theme-text dark:text-dark-text flex items-center gap-3 sm:gap-4 ml-4">
        <a
          v-if="appConfig.socials?.twitter || appConfig.socials?.x"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://x.com/${appConfig.socials?.twitter || appConfig.socials?.x}`"
          title="X"
        >
          <Icon class="size-5" name="i-simple-icons-x" />
        </a>
        <a
          v-if="appConfig.socials?.github"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://github.com/${appConfig.socials?.github}`"
          title="GitHub"
        >
          <Icon class="size-5" name="i-simple-icons-github" />
        </a>
        <a
          v-if="appConfig.socials?.mastodon"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://elk.zone/${appConfig.socials?.mastodon}`" title="Mastodon"
        >
          <Icon class="size-5" name="i-simple-icons-mastodon" />
        </a>
        <a
          v-if="appConfig.socials?.bluesky"
          class="hover:text-black dark:hover:text-white transition-colors"
          :href="`https://bsky.app/profile/${appConfig.socials?.bluesky}`" title="Bluesky"
        >
          <Icon class="size-5" name="i-simple-icons-bluesky" />
        </a>
        <ColorModeSwitch class="hover:text-black dark:hover:text-white transition-colors" />
      </div>
      
    </div>
  </div>
</template>
