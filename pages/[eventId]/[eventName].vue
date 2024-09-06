<template>
  <div v-if="event">
    <div class="h-[500px] w-full overflow-hidden relative">
      <img
        :src="eventImage"
        class="object-cover h-full w-screen object-center"
      />
      <div
        class="bg-[#b7ead6] h-fit w-full absolute bottom-0 z-90 opacity-95 p-5 flex flex-col gap-4"
      >
        <h1 class="text-4xl">{{ event.title }}</h1>
        <div class="flex md:items-center gap-3">
          <UIcon name="i-heroicons-calendar-solid" class="bg-black w-7 h-7" />
          <div>
            {{ formatEventDate(event.start).eventDate }}
            <span class="text-sm text-gray-900">{{
              formatEventDate(event.start).eventTime
            }}</span>
          </div>
          -
          <div>
            {{ formatEventDate(event.end).eventDate }}
            <span class="text-sm text-gray-900">{{
              formatEventDate(event.end).eventTime
            }}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="flex flex-col gap-6 px-10 py-10">
      <h3 class="text-2xl underline">About</h3>
      <div v-html="event.description" />
    </div>
  </div>
</template>

<script setup>
import dayjs from "#build/dayjs.imports.mjs";

// Get eventId and eventName
const {
  params: { eventId, eventName },
} = useRoute();

const event = ref();
const eventImage = ref("");

// Fetch single event - based off eventId in the URL.
const fetchEvent = async () => {
  try {
    const response = await fetch("/api/events-data.json");
    const data = await response.json();
    const foundEvent = data.find((event) => event.id === Number(eventId));
    event.value = foundEvent;
  } catch (e) {
    console.error("error fetching events: ", e);
  }
};

// Split and Format date/time to be more palatable.
const formatEventDate = (date) => {
  const eventDate = dayjs(date).format("ddd DD, MMM YYYY");
  const eventTime = dayjs(date).format("HH:mm");

  return {
    eventDate,
    eventTime,
  };
};

// Nuxt was being a pain with images, so had to import it this way.
watchEffect(async () => {
  if (eventId) {
    try {
      const imageModule = await import(`@/public/images/${eventId}.webp`);
      eventImage.value = imageModule.default;
    } catch (e) {
      console.error("error loading image: ", e);
    }
  }
});

// Get Event
onMounted(async () => {
  await fetchEvent();
});
</script>
