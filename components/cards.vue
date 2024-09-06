<template>
          <div
        class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-x-6 gap-y-6 sm:gap-x-8 sm:gap-y-4 md:gap-x-10 md:gap-y-5 px-6"
      >
        <UCard
          v-for="(event, index) in events"
          :key="index"
          :ui="cardUi"
          class="hover:scale-105 cursor-pointer"
          @click="navigateToEvent(event)"
        >
          <template #header>
            <img :src="event.image" />
          </template>

          <template #footer>
            <div class="mb-4">
              <h2 class="text-lg font-semibold">{{ event.title }}</h2>
            </div>
            <div class="flex items-center gap-2">
              <UIcon
                name="i-heroicons-calendar-solid"
                class="text-[#34cc98] w-6 h-6"
              />
              {{ formatEventDate(event.start).startDate }}
              <span class="text-sm text-gray-500">{{
                formatEventDate(event.start).startTime
              }}</span>
            </div>
            <div class="flex justify-end mt-2">
              <UButton
                class="rounded-full px-8 bg-[#222634] hover:bg-[#34cc98]"
                size="xl"
                >More Info</UButton
              >
            </div>
          </template>
        </UCard>
      </div>
</template>

<script setup>
import dayjs from "#build/dayjs.imports.mjs";

const props = defineProps({
    events: Array,
})

// Styling config for Card Component
const cardUi = {
  base: "overflow-hidden cutout relative",
  ring: "",
  shadow: "",
  body: { padding: "" },
  header: { padding: "" },
};

// Split/Format Date + Time
const formatEventDate = (start) => {
  const startDate = dayjs(start).format("ddd DD, MMM YYYY");
  const startTime = dayjs(start).format("HH:mm");

  return {
    startDate,
    startTime,
  };
};

// When card is clicked on, take user to that event page.
const navigateToEvent = (event) => {
  const eventId = event.id;
  const eventName = event.title;
  navigateTo({
    name: "eventId-eventName",
    params: { eventId, eventName },
  });
};
</script>

<style lang="scss">
.cutout {
  &::before {
    content: "";
    width: 60px;
    height: 60px;
    background: #b7ead6;
    border-radius: 100px;
    position: absolute;
    left: 0;
    right: 0;
    margin: auto;
    top: -30px;
    z-index: 1;
  }
}
</style>