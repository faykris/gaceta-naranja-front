<template>
  <q-page class="q-px-lg q-py-lg">
    <div class="q-px-lg q-py-lg bg-white shadow-4 rounded">
      <h3 class="text-center q-mt-md q-mb-none" >¡Lo sentimos!</h3>
      <div class="flex items-center justify-center wrap q-px-lg q-py-lg">
        <img class="img-404" src="src/assets/images/404image.png" alt="404" srcset="">
      </div>
      <h6 class="text-center text-secondary q-mt-none q-mb-sm">
        Este recurso no se encuentra o no está disponible
      </h6>
      <div class="flex items-center justify-center">
        <q-btn
        class="q-mt-sm q-mb-lg"
        color="primary"
        text-color="white"
        unelevated
        to="/"
        label="Volver al Inicio"
        no-caps
      />
      </div>
    </div>
  </q-page>
</template>

<script lang="ts" setup>
</script>

<style lang="scss" scoped>
.img-404 {
  width: 25vw;
}
.rounded {
  border-radius: 10px;
}
</style>
