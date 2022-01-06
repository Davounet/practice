<template>
  <div>
    <h2>Récupération de données depuis une API</h2>
    <button type="button" class="form-button" @click="load">Afficher un chat</button>
    <div v-if="status === 'loading'" class="flex items-center justify-center w-80 h-80">
      <svg
        class="w-20 h-20 text-primary-800 animate-spin"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
      >
        <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
        <path
          class="opacity-75"
          fill="currentColor"
          d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
        ></path>
      </svg>
    </div>
    <img v-if="image && status === 'idle'" :src="image" alt="un chat" class="object-cover mt-2 rounded-lg h-80" />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'ImageApiComponent',
  data() {
    return {
      status: 'idle',
      image: '',
      cache: 0
    }
  },
  methods: {
    async load(): Promise<void> {
      try {
        this.status = 'loading'
        const response = await this.$axios.$get('/cat', {
          responseType: 'arraybuffer',
          params: { cache: this.cache }
        })
        const base64 = Buffer.from(response, 'binary').toString('base64')
        this.image = `data:image/jpg;base64,${base64}`
        this.status = 'idle'
        this.cache++
      } catch (e) {
        this.status = 'error'
      }
    }
  }
})
</script>
