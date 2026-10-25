<template>
  <!-- 把 max-w-2xl 删掉，换成 w-full，让它在 80% 的容器里完全撑开 -->
  <div class="px-4 py-10 m-auto sm:px-8 sm:rounded-lg w-full sm:shadow">
    <main class="max-w-none">
      <slot>
        <h1 class="text-2xl">Page not found</h1>
      </slot>
    </main>
  </div>
</template>
