<template>
  <div>
    <h1>Esto en el articulo numero: {{ myId }}</h1>
  </div>
</template>
<script setup>
const route = useRoute()
const myId = ref(route.params.id);

</script>
