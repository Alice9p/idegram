<template>
  <div class="
               d-flex
               fixed-top
               vw-100
               vh-100
               bg-light
               justify-content-center
               align-items-center
               opacity-75
             "
       :class="{ 'd-none': !show }"
  >
    <div class="spinner-border" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
  </div>
</template>
<script setup>
const props = defineProps({
  show: Boolean
})
</script>