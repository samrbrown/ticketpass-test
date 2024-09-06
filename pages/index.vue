<template>
  <div class="h-full flex flex-col">
    <div class="flex-grow py-6">
      <Cards :events="events" />
    </div>
    <Pagination :page="page" :totalPages="totalPages" @change="changePage" />
  </div>
</template>

<script setup>
definePageMeta({
  title: "Ticketpass - Events",
  description: "Here you can find the current list of upcoming events.",
});

const page = ref(1);
const limit = ref(6);
const totalPages = ref(1);
const events = ref([]);
const allEvents = ref([]);

// Fetch All Events from the API
const fetchEvents = async () => {
  try {
    const response = await fetch("/api/events-data.json");
    const data = await response.json();
    allEvents.value = data;
    totalPages.value = Math.ceil(allEvents.value.length / limit.value);
    updateEvents();
  } catch (e) {
    console.error("error fetching events: ", e);
  }
};

// Update shown events based on pagination
const updateEvents = () => {
  const startIndex = (page.value - 1) * limit.value;
  const endIndex = page.value * limit.value;
  events.value = allEvents.value.slice(startIndex, endIndex);
};

watch(page, fetchEvents);

// For pagination - waits for pagination component to emit.
const changePage = (newPage) => {
  page.value = newPage;
};

onMounted(async () => {
  await fetchEvents();
})

</script>
