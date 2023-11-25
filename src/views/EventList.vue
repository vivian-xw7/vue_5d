<script>
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'
import NProgress from 'nprogress'

export default {
  name: 'EventList',
  props: ['page'],
  components: {
    EventCard
  },
  data() {
    return {
      events: null,
      totalEvents: 0
    }
  },
  beforeRouteEnter(routeTo, routeFrom, next) {
    NProgress.start()
    EventService.getEvents(2, parseInt(routeTo.query.page || 1))
      .then(response => {
        next(comp => {
          comp.events = response.data
          comp.totalEvents = response.headers['x-total-count']
        })
      })
      .catch(() => {
        next({ name: 'NetworkError' })
      })
      .finally(() => {
        NProgress.done()
      })
  },
  beforeRouteUpdate(routeTo) {
    NProgress.start()
    EventService.getEvents(2, parseInt(routeTo.query.page || 1))
      .then(response => {
        this.events = response.data
        this.totalEvents = response.headers['x-total-count']
      })
      .catch(() => {
        return { name: 'NetworkError' }
      })
      .finally(() => {
        NProgress.done()
      })
  },
  computed: {
    hasNextPage() {
      var totalPages = Math.ceil(this.totalEvents / 2)

      return this.page < totalPages
    },
    totalPages() {
      return Math.ceil(this.totalEvents / 2)
    }
  }
}
</script>

<template>
  <h1>Events for Good</h1>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <div class="pagination">
      <router-link
        id="prev-page"
        :to="{ name: 'EventList', query: { page: page - 1 } }"
        rel="prev"
        v-if="page != 1"
        >Previous Page</router-link
      >

      <router-link
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        :to="{ name: 'EventList', query: { page: pageNumber } }"
        :class="{ active: page === pageNumber }"
        rel="page-number"
      >
        {{ pageNumber }}
      </router-link>

      <router-link
        id="next-page"
        :to="{ name: 'EventList', query: { page: page + 1 } }"
        rel="next"
        v-if="hasNextPage"
        >Next Page</router-link
      >
    </div>
  </div>
</template>


<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
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
