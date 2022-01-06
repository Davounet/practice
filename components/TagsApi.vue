<template>
  <div>
    <h2>Récupération de données JSON depuis une API</h2>
    <form class="w-full max-w-sm" @submit.prevent="submit">
      <label for="tags" class="block text-sm font-medium text-gray-700">Filtrer les chats par tag</label>
      <div class="flex mt-1 rounded-md shadow-sm">
        <input
          id="tags"
          v-model="tag"
          type="tags"
          name="tags"
          class="rounded-r-none shadow-none form-input focus:z-10"
          placeholder="cute"
        />
        <button type="submit" class="relative -ml-px rounded-l-none form-button">Filtrer</button>
      </div>
    </form>
    <div class="mt-2 overflow-y-auto rounded-lg bg-gray-50 max-h-80">
      <div class="px-4 py-5 sm:p-6">
        <svg
          v-if="status === 'loading'"
          class="w-5 h-5 text-primary-800 animate-spin"
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
        <pre v-if="status === 'idle'" class="text-xs">{{ results || 'Aucun résultat' }}</pre>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'JsonApiComponent',
  data() {
    return {
      tag: '',
      status: 'idle',
      results: []
    }
  },
  methods: {
    async submit(): Promise<void> {
      this.status = 'loading'
      const response = await this.$axios.$get('/api/cats', { params: { tags: this.tag } })
      this.results = response
      this.status = 'idle'
    }
  }
})
</script>

focus:ring-indigo-500 focus:border-indigo-500 block w-full rounded-none rounded-l-md pl-10 sm:text-sm border-gray-300
