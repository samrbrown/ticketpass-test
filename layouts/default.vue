<template>
  <div class="flex flex-col items-center w-full h-full">
    <Header />
    <div
      class="flex flex-col m-0 w-full flex-grow"
      :class="route.path === '/' ? 'bg-[#b7ead6]' : ''"
    >
      <div class="flex flex-col h-full">
        <slot />
      </div>
    </div>
  </div>
</template>

<script setup>
const route = useRoute();
</script>
