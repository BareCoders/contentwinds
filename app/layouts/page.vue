<template>
  <!-- 外层卡片保持 w-full，撑满 80% 的屏幕 -->
  <div class="px-4 py-10 m-auto sm:px-8 sm:rounded-lg w-full sm:shadow">
    
    <!-- 这里改成了 max-w-4xl，让文字限制在 896 像素并居中 -->
    <main class="px-4 mx-auto max-w-4xl prose">
      <slot>
        <h1 class="text-2xl">Page not found</h1>
      </slot>
    </main>
    
  </div>
</template>
