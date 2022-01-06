<template>
  <div class="flex flex-col justify-center min-h-screen py-12 bg-primary-200 sm:px-6 lg:px-8">
    <div class="w-full mt-6 mb-4 sm:mx-auto sm:max-w-screen-lg">
      <div class="px-4 py-8 bg-white shadow-md sm:rounded-lg sm:px-10">
        <nuxt />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  name: 'DefaultLayout'
})
</script>
