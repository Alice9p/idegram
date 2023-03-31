<template>
  <article class="mb-3">
    <!-- Imagen -->
    <div class="ratio ratio-4x3">
      <img :src="urlPublic" alt="" class="
            img
            object-fit-cover
            border
            rounded
            ">
    </div>
    <!-- Controles -->
    <div class="d-flex justify-content-end">
      <button @click="addLike" class="btn btn-outline-primary p-3" aria-current="page">❤ {{ likes }}</button>
      <button class="btn btn-outline-primary p-3">💭</button>

    </div>
  </article>
</template>

<script setup>

import { createClient } from '@supabase/supabase-js'

//Variables

const supabase = createClient('https://fenyqczbljoqzhokzbkz.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImZlbnlxY3pibGpvcXpob2t6Ymt6Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2Nzk1NjA4ODcsImV4cCI6MTk5NTEzNjg4N30.XCPNG19OcO2OHdiCAAgYkbfy3E7WwGXrgG-imZy8M9E')
const urlPublic = ref();

const props = defineProps({
  id: Number,
  url: String,
  likes: Number
})


onMounted(async() => {
  // Obtener la URL publica de la imagen
  const { data }  = await supabase
      .storage
      .from('photos')
      .getPublicUrl(props.url);
  urlPublic.value = data.publicUrl;
})

async function addLike () {
  const { error } = await supabase
      .from('photos')
      .update({ likes: props.likes + 1 })
      .eq('id', props.id)

}

</script>

<style scope>
.img {
  width: 100%;
}
</style>