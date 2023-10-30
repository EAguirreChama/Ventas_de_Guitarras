<script setup>
// Este es el componente hijo
// Props que viene desde App.vue
const props = defineProps({
    guitarra: {
        type: Object,   // Tipo de dato que se pasa
        required: true  // Si es requerido
    }
})

defineEmits(['agregar-carrito'])
</script>

<template>
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">

            <!-- Atributos dinámicos -->
            <img class="img-fluid" :src="'/img/' + guitarra.imagen + '.jpg'" :alt="'imagen guitarra' + guitarra.nombre">

        </div>

        <div class="col-8">
            <!-- Se muestra el nombre de cada guitarra-->
            <h3 class="text-black fs-4 fw-bold text-uppercase"> {{ guitarra.nombre }} </h3>

            <!-- Se muestra la descripción de cada guitarra-->
            <p> {{ guitarra.descripcion }} </p>

            <!-- Se muestra el precio de cada guitarra -->
            <p class="fw-black text-primary fs-3">${{ guitarra.precio }}</p>

            <button type="button" class="btn btn-dark w-100 " @click="$emit('agregar-carrito', guitarra)">
                Agregar al Carrito
            </button>
        </div>
    </div>
</template>

<style lang="scss" scoped></style>