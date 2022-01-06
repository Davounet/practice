<template>
  <div>
    <h2>Utilisation d'un éditeur de texte riche</h2>
    <client-only>
      <TipTap v-model="content" type="html" />
    </client-only>

    <div class="mt-2 overflow-y-auto rounded-lg bg-gray-50 max-h-80">
      <div class="px-4 py-5 sm:p-6">
        <pre class="text-xs">{{ content }}</pre>
      </div>
    </div>

    <div class="mt-2 overflow-y-auto rounded-lg bg-gray-50 max-h-80">
      <div class="px-4 py-5 sm:p-6">
        <!-- eslint-disable-next-line vue/no-v-html -->
        <div class="prose" v-html="content" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'RichEditorComponent',
  data() {
    return {
      content: ''
    }
  }
})
</script>
