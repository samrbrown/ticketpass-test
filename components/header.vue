<template>
  <div
    class="mt-4 rounded-xl py-5 flex items-center bg-white w-full px-4 md:px-10"
  >
    <div class="flex-grow md:shrink-0 md:flex-grow-0">
      <NuxtLink to="/">
        <img src="~/public/ticketpass_logo.232ef43.svg" class="w-48" />
      </NuxtLink>
    </div>
    <div class="hidden md:flex">
      <UHorizontalNavigation :links="links" class="px-20" />
    </div>
    <div class="md:hidden">
      <UButton
        icon="i-heroicons-bars-3"
        variant="outline"
        size="xl"
        @click="isOpen = true"
      />
    </div>
    <USlideover v-model="isOpen">
      <UCard
        class="flex flex-col flex-1"
        :ui="{
          body: { base: 'flex-1' },
          ring: '',
          divide: 'divide-y divide-gray-100 dark:divide-gray-800',
        }"
      >
        <UButton
          color="gray"
          variant="ghost"
          size="sm"
          icon="i-heroicons-x-mark-20-solid"
          class="flex sm:hidden absolute end-5 top-1 z-10"
          square
          padded
          @click="isOpen = false"
        />
        <div class="h-full py-10">
          <UVerticalNavigation :links="links">
            <template #default="{ link }">
              <span
                class="group-hover:text-[#34cc98] relative text-2xl"
                @click="isOpen = false"
                >{{ link.label }}</span
              >
            </template>
          </UVerticalNavigation>
        </div>

        <template #footer>
          <Placeholder class="h-8" />
        </template>
      </UCard>
    </USlideover>
  </div>
</template>

<script setup>
const isOpen = ref(false);
const links = [{ label: "Events", to: "/" }];
</script>
