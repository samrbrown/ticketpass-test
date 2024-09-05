<template>
  <div class="h-full flex flex-col">
    <div class="flex-grow py-6">
      <Cards :events="events" />
    </div>
    <Pagination :page="page" :totalPages="totalPages" @change="changePage" />
  </div>
</template>

<script setup>

const page = ref(1);
const limit = ref(6);
const totalPages = ref(1);
const events = ref([]);
const allEvents = ref([]);

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

const updateEvents = () => {
  const startIndex = (page.value - 1) * limit.value;
  const endIndex = page.value * limit.value;
  events.value = allEvents.value.slice(startIndex, endIndex);
};

watch(page, fetchEvents);

const changePage = (newPage) => {
  page.value = newPage;
};

fetchEvents();
</script>
