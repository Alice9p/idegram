<template>
  <div>
    <head>
      <Title>Idegram</Title>
      <Meta name="description" :content="title" />
      <Link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous" />
    </head>
    <div class="custom-container">
      <header>
        <h1 class="text-center mt-5">Idegram</h1>
      </header>
      <main>
        <slot />
      </main>
      <footer class="my-5 text-center">2023</footer>
    </div>
  </div>
</template>

<style scope>
.custom-container {
  padding: 1rem;
  margin: 0 auto;
  max-width: 40rem;
}
</style>
