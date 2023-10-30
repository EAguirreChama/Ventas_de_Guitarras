<script setup>
// Este es el componente padre
import { ref, reactive, onMounted, watch } from "vue"
import { db } from "./data/guitarras"

import Guitarra from "./components/Guitarra.vue" // Componente donde aparecen todas las guitarras
import Header from "./components/Header.vue" //  Componente donde esta el carrito
import Footer from "./components/Footer.vue" // Componente Footer

// const state = reactive ({
//     guitarras: db
// })

// STATES
const guitarras = ref([])  // State de Guitarras (aquí se encuentran todas las guitarras)
const carrito = ref([])    // State de Carrito de Compras (aquí se agregan las guitarras)
const guitarra = ref({})   // State de una Guitarra

// Función agregarCarrito para pasar como un event
const agregarCarrito = (guitarra) => {
    // Se verifica si una guitarra se encuentra en el carrito
    const exiteEnCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)

    if (exiteEnCarrito >= 0) {
        carrito.value[exiteEnCarrito].cantidad++
    } else {
        // La cantidad con la que se inicia
        guitarra.cantidad = 1;

        // Se agrega al state de carrito (un arreglo) la guitarra
        carrito.value.push(guitarra);
    }
}

watch(carrito, () => {
    guardarCarrito()
}, {
    deep: true
})

onMounted(() => {
    guitarras.value = db;
    guitarra.value = db[3]

    const carritoStorage = localStorage.getItem('carrito')
    if (carritoStorage) {
        carrito.value = JSON.parse(carritoStorage)
    }
})

const guardarCarrito = () => {
    localStorage.setItem("carrito", JSON.stringify(carrito.value))
};

const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    if (carrito.value[index].cantidad <= 1) return
    carrito.value[index].cantidad--
};

const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    if (carrito.value[index].cantidad >= 5) return
    carrito.value[index].cantidad++
};

const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id)
};

const vaciarCarrito = () => {
    carrito.value = []
};
</script>

<template>
    <Header :carrito="carrito" :guitarra="guitarra" @decrementar-cantidad="decrementarCantidad"
        @incrementar-cantidad="incrementarCantidad" @agregar-carrito="agregarCarrito" @eliminar-Producto="eliminarProducto"
        @vaciar-carrito="vaciarCarrito" />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <!-- Se renderiza el componente -->

            <!-- Se hace la iteración del componente con v-for -->
            <!-- Props guitarra para que vaya al componente Guitarra.vue -->
            <!-- Event para agregar carrito  -->
            <Guitarra v-for="guitarra in guitarras" :guitarra="guitarra" @agregar-carrito="agregarCarrito"
                :key="guitarra.id" />
        </div>
    </main>
    <Footer />
</template>
