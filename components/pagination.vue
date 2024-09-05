<template>
  <div
    class="mt-5 flex gap-4 items-center justify-center items-stretch bg-white w-full z-10 sticky bottom-0 left-0 py-5 px-5"
  >
    <UButton
      :disabled="page === 1"
      icon="i-heroicons-arrow-long-left-20-solid"
      color="white"
      @click="prevPage"
    />
    <div
      v-for="n in totalPages"
      :key="n"
      class="bg-white rounded min-w-10 flex items-center justify-center ring-1 ring-gray-200 cursor-pointer"
      :class="{ '!bg-[#34cc98] text-white': n === page }"
      @click="page = n"
    >
      {{ n }}
    </div>
    <UButton
      icon="i-heroicons-arrow-long-right-20-solid"
      color="white"
      @click="nextPage"
      :disabled="page === totalPages"
    />
  </div>
</template>

<script setup>
const page = defineModel("page");
const totalPages = defineModel("totalPages");
const emit = defineEmits(["change"]);

const nextPage = () => {
  if (page.value < totalPages.value) {
    page.value++;
  }
};

const prevPage = () => {
  if (page.value > 1) {
    page.value--;
  }
};

watch(page, (newPage) => {
  if (newPage) {
    emit("change", newPage);
  }
});
</script>
