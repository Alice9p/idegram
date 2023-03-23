<template>
  <hr/>
  <p>
    <strong>Nombre:</strong> {{ name }}
  </p>
  <hr/>
</template>

<script setup>
const props = defineProps({
  name: String
})
</script>