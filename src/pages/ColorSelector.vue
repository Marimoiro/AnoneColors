<script setup lang="ts">

const props = defineProps({
  color: {
    type: String,
    default: 'white'
  },
  name: {
    type: String,
    default: 'color-selector'
  },
  index: {
    type: Number,
    default: 0
  },
  onSelect: {
    type: Function,
    default: () => {}
  }
})


</script>

<template>
  <q-btn :color="props.color" @click="props.onSelect!">

  </q-btn>

</template>

<style scoped>

</style>
