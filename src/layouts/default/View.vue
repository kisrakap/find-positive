<template>
  <v-main class="d-flex flex-column justify-center" style="height: 100%">
    <Header />
    <div class="flex-grow-1 overflow-auto">
      <router-view />
    </div>
    <Footer> </Footer>
  </v-main>
</template>

<script setup>
//
import Footer from "@/components/Footer.vue";
import Header from "@/components/Header.vue";

import {} from "module";
</script>

<style scoped>
.overflow-auto {
  max-height: calc(
    100vh - 64px
  ); /* Adjust the max-height as needed, considering the height of the Header */
}
</style>
