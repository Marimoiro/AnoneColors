<script setup lang="ts">


const props = defineProps({
  baseColor: {
    type: String,
    default: 'white'
  },
  subColor: {
    type: String,
    default: 'black'
  },
  title: {
    type: String,
    default: 'title'
  }

})

</script>



<template>

  <!-- 円を描く -->
  <svg width="inherit" height="inherit" viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="48" :stroke="props.baseColor" stroke-width="2" :fill="props.baseColor" />
    <line x1="5" y1="105" x2="105" y2="5" :stroke="props.subColor" stroke-width="5" />
    <line x1="-5" y1="95" x2="95" y2="-5" :stroke="props.subColor" stroke-width="5" />
  </svg>

</template>

<style scoped>

/* cssで円を描く */
.circle {
  width: inherit;
  height: inherit;
  border-radius: 50% !important;
  background-color: #f00;
}

.circle::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(-45deg, transparent 49%, black 50%, transparent 51%);
}


</style>
