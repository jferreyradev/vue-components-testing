<script setup>
import { useStore } from '../composables/useStore.js'

const store = useStore()

</script>

<template>
  <div class="about">
    <h1>This is an about page</h1>
    <pre>
      {{ store.globalState }}
    </pre>
  </div>
</template>

<style></style>
