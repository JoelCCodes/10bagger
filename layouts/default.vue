<template>
  <div>
    <nav
      class="navbar header has-shadow is-primary"
      role="navigation"
      aria-label="main navigation"
    >
      <div class="navbar-brand">
        <a class="navbar-item" href="/">
          <span>10Bagger.com</span>
        </a>
      </div>
    </nav>

    <section class="main-content columns">
      <aside class="column is-2 section">
        <p class="menu-label is-hidden-touch">General</p>
        <ul class="menu-list">
          <li v-for="(item, key) of items" :key="key">
            <nuxt-link :to="item.to" exact-active-class="is-active">
              <b-icon :icon="item.icon" /> {{ item.title }}
            </nuxt-link>
          </li>
        </ul>
      </aside>

      <div class="container column is-10">
        <nuxt />
      </div>
    </section>
  </div>
</template>
<script>
import Vue from 'vue'

import VueAnalytics from 'vue-analytics'

Vue.use(VueAnalytics, {
  id: 'G-SYP6CRQW0X',
})

export default {
  mounted() {
    this.$ga.page('/')
  },
  data() {
    return {
      items: [
        {
          title: 'Home',
          icon: 'home',
          to: { name: 'index' },
        },
        {
          title: 'About',
          icon: 'lightbulb',
          to: { name: 'inspire' },
        },
      ],
    }
  },
}
</script>
