<template>
  <div id="events-app">
    <div class="spinner" v-if="loading">
      <div class="rect1"></div>
      <div class="rect2"></div>
      <div class="rect3"></div>
      <div class="rect4"></div>
      <div class="rect5"></div>
    </div>
    <div v-if="!loading">
      <div class="row is-multiline">
        <div class="col is-12" v-if="events.length === 0">
          <p>There are no upcoming events.</p>
        </div>
        <div class="col is-12" v-for="event of events" :key="event.id">
          <a :href="event.link" target="_blank">
          <div class="sgds-card sgds-card-button">
            <div class="sgds-card-header">
              <p class="sgds-card-header-title">{{ event.name }}</p>
            </div>
            <div class="sgds-card-content">
              <div class="row">
                <div class="col is-2 has-text-centered">
                  <p>{{ event.time | moment("MMM") }}</p>
                  <h2>{{ event.time | moment("D") }}</h2>
                </div>
                <div class="col is-10">
                  <p class="summary">{{ event.plain_text_description }}</p>
                  <br />
                  <small>
                    <span class="sgds-icon sgds-icon-my-location"></span>
                    {{ event.venue.name }}
                  </small>
                </div>
              </div>
            </div>
          </div>
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { apiClient } from '../lib';
export default {
  data() {
    return {
      events: [],
      loading: false
    };
  },
  created() {
    this.loading = true;
    apiClient
      .get("/events")
      .then(response => {
        this.events = response.data;
      })
      .finally(() => {
        this.loading = false;
      });
  }
};
</script>

<style scoped>
.summary {
  max-height: 100px;
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  display: -webkit-box;
}
</style>
