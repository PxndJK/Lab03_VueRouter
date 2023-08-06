<script setup lang="ts">
import EventCard from '../components/EventCard.vue'
import EventCard2 from '../components/EventCard2.vue'
import type { EventItem } from '@/type'
import { ref, type Ref } from 'vue'
// import axios from 'axios'
import EventService from '@/services/EventService'
import type { AxiosResponse } from 'axios'

const events: Ref<Array<EventItem>> = ref([])

// axios.get<EventItem[]>('http://localhost:3004/events')
// .then((response) => {
//   events.value = response.data
// })
const props = defineProps({
  page: {
    type: Number,
    required: true
  }
})
EventService.getEvent(2, props.page).then((response: AxiosResponse<EventItem[]>) =>{
  events.value = response.data
})
</script>

<template>
  <h1>Events For Good</h1>
  <main class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event"></EventCard>
    <RouterLink :to="{name: 'event-list', query: {page: page - 1}}" rel="prev" v-if="page!= 1">
      Prev Page
    </RouterLink>
    <RouterLink :to="{name: 'event-list', query: {page: page + 1}}" rel="next" v-if="page + 1">
      Next Page
    </RouterLink>
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
</style>
