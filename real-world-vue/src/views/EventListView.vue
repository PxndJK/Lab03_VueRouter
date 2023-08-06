<script setup lang="ts">
import EventCard from '../components/EventCard.vue'
import EventCard2 from '../components/EventCard2.vue'
import type { EventItem } from '@/type'
import { computed, ref, type Ref, watchEffect } from 'vue'
// import axios from 'axios'
import EventService from '@/services/EventService'
import type { AxiosResponse } from 'axios'
// import { watchEffect } from 'vue'

const events: Ref<Array<EventItem>> = ref([])

// axios.get<EventItem[]>('http://localhost:3004/events')
// .then((response) => {
//   events.value = response.data
// })
const totalEvent = ref<number>(0)
const props = defineProps({
  page: {
    type: Number,
    required: true
  }
})

EventService.getEvent(2, props.page).then((response: AxiosResponse<EventItem[]>) => {
  events.value = response.data
})

watchEffect(() => {
  EventService.getEvent(2, props.page).then((response: AxiosResponse<EventItem[]>) => {
    events.value = response.data
    totalEvent.value = response.headers['x-total-count']
  })
})

const hasNextPage = computed(() => {
  const totalPages = Math.ceil(totalEvent.value / 2)
  return props.page.valueOf() < totalPages
})
</script>

<template>
  <h1>Events For Good</h1>
  <main class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event"></EventCard>
    <div class="pagination">
      <RouterLink
        :to="{ name: 'EventList', query: { page: page - 1 } }"
        rel="prev"
        v-if="page != 1"
      >
        Prev Page
      </RouterLink>
      <RouterLink
        :to="{ name: 'EventList', query: { page: page + 1 } }"
        rel="next"
        v-if="hasNextPage"
      >
        Next Page
      </RouterLink>
    </div>
  </main>
  <!-- <mian class="events2">
    <EventCard2 v-for="event in events" :key="event.id" :event="event"></EventCard2>
  </mian> -->
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.events2 {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: right;
}

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
