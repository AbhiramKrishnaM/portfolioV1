<template>
  <v-app>
    <!-- main content -->
    <v-main>
      <Nuxt />
    </v-main>
    <!-- end of main content -->
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      fixed: false,
    }
  },
}
</script>
