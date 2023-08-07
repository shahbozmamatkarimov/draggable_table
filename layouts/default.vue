<template>
  <header class="flex">
    <Sidebar class="w-56" />
    <div class="w-full">
      <div class="overflow-hidden border w-full overflow-y-auto max-h-screen">
        <slot />
      </div>
    </div>
  </header>
</template>

<script setup></script>

<style lang="scss" scoped></style>
