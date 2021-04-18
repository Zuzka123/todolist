<template>
  <v-app>
    <p>
      <router-link to="/">
        Go to Home
      </router-link>
      |
      <router-link to="/about">
        Go to About
      </router-link>
    </p>
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App'
}
</script>
