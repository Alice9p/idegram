<template>
  <div>
    <Loading :show="verLoading" />
    <button type="button" class="btn btn-success" @click="showModalNewPhoto = true">➕</button>
    <!-- Listado de imagenes -->
    <h1>Listado de imagenes subidas</h1>
    <Card v-for="photo in photos" :url="photo.url" :likes="photo.likes" :id="photo.id"/>
    <!-- Modal para subir una nueva imagen -->
    <div class="modal" tabindex="-1" :class="{'d-block' : showModalNewPhoto}">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Subir imagen</h5>
            <button @click="showModalNewPhoto = false" type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <input ref="inputFile" type="file">
          </div>
          <div class="modal-footer">
            <button @click="showModalNewPhoto = false" type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
            <button type="button" class="btn btn-primary" @click="subirFoto">Subir</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>


import { createClient } from '@supabase/supabase-js'

//Variables

const supabase = createClient('https://fenyqczbljoqzhokzbkz.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImZlbnlxY3pibGpvcXpob2t6Ymt6Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2Nzk1NjA4ODcsImV4cCI6MTk5NTEzNjg4N30.XCPNG19OcO2OHdiCAAgYkbfy3E7WwGXrgG-imZy8M9E')
const inputFile = ref();
const photos = ref();
const showModalNewPhoto = ref(false);
const verLoading = ref(false);

// Funciones
async function obtenerFotos() {
  const { data, error } = await supabase
      .from('photos')
      .select()
      .order('id', { ascending: false })
  photos.value = data;

}

async function vigilarCambios() {
  supabase
      .channel('any')
      .on('postgres_changes', { event: 'UPDATE', schema: 'public', table: 'photos' }, payload => {
        obtenerFotos();
      })
      .subscribe()
}

async function subirFoto() {
  photos.value = [];
  // Ver loading
  verLoading.value = true;
// Cerramos modal
  showModalNewPhoto.value = false;
  // Generamos la ruta de la imagen
  const url = `public/${crypto.randomUUID()}.jpg`;
// Subimos la imagen al bucket (disco duro online)
  const { data, errorUpload } = await supabase
      .storage
      .from('photos')
      .upload(url, inputFile.value.files[0], {
        cacheControl: '3600',
        upsert: false
      });

  // Guardamos la ruta en la base de datos

  const { errorTable } = await supabase
      .from('photos')
      .insert({ url: url, id_user: 'anonimo' })

  // Refrescamos las fotos que se muesrtan
   await obtenerFotos();
  // Quitar loading
  verLoading.value = false;

}

// Cuando se cargue la pagina, llamaré todo el contenido de onMounted
onMounted(async() => {
  // Ver loading
  verLoading.value = true;
  // Obtiene las fotos
  await obtenerFotos();
  // Ocultar loading
  verLoading.value = false
  // Vigilamos cambios en los likes
  vigilarCambios();
})


const { data: users } = await useFetch('https://jsonplaceholder.typicode.com/users')

</script>