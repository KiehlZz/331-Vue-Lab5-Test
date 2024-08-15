<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import StudentList from '@/components/StudentList.vue'
import { type Event } from '@/types'
import { ref, onMounted, computed, watchEffect } from 'vue'
import EventService from '@/services/EventService'

const events = ref<Event[] | null>(null)
const totalEvents = ref(0)

const props = defineProps({
  page: {
    type: Number,
    required: true
  },
  pageSize: {
    type: Number,
    required: true
  }
})

const page = computed(() => props.page)
const pageSize = computed(() => props.pageSize)

const hasNextPage = computed(() => {
  const totalPage = Math.ceil(totalEvents.value / pageSize.value)
  return page.value < totalPage
})

onMounted(() => {
  watchEffect(() => {
    EventService.getEvents(pageSize.value, page.value)
      .then((response) => {
        events.value = response.data
        totalEvents.value = response.headers['x-total-count']
      })
      .catch((error) => {
        console.error('There was an error!', error)
      })
  })
})
</script>

<template>
  <h1 class="text-center text-2xl font-bold mb-4">Event For Good</h1>
  <div class="flex flex-col items-center">
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <StudentList v-for="event in events" :key="event.id" :event="event" />
    <div class="flex w-72 justify-between mt-4">
      <RouterLink id="page-prev" :to="{ name: 'event-list-view', query: { page: page - 1, pageSize: pageSize } }" rel="prev" v-if="page != 1"
      class="text-blue-500 hover:text-blue-700 text-left flex-1">&#60; Prev Page</RouterLink>
      <RouterLink id="page-next" :to="{ name: 'event-list-view', query: { page: page + 1, pageSize: pageSize } }" rel="next" v-if="hasNextPage"
      class="text-blue-500 hover:text-blue-700 text-left flex-1">Next Page &#62;</RouterLink>
    </div>
  </div>
</template>

<style scoped>
.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}
#page-prev {
  text-align: left;
}
#page-next {
  text-align: right;
}
</style>
