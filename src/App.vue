<script setup>

    import { ref, reactive, onMounted, watch } from "vue"
    import { db } from "./data/guitarras"
    import Guitarra from "./components/Guitarra.vue"
    import Header from "./components/Header.vue"
    import Footer from "./components/Footer.vue"

    // const state = reactive ({
    //     guitarras: db
    // })

    const guitarras = ref([])
    const carrito = ref([])
    const guitarra = ref({})

    watch(carrito, () => {
        guardarCarrito()
    }, {
        deep: true
    })

    onMounted(() => {
        guitarras.value = db;
        guitarra.value = db[3]
        
        const carritoStorage = localStorage.getItem('carrito')
        if(carritoStorage){
            carrito.value = JSON.parse(carritoStorage)
        }
    })

    const guardarCarrito = () => {
        localStorage.setItem("carrito", JSON.stringify(carrito.value))
    }

    const agregarCarrito = (guitarra) => {
        const exiteEnCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)

        if(exiteEnCarrito >= 0) {
            carrito.value[exiteEnCarrito].cantidad++
        } else {
            guitarra.cantidad = 1;
            carrito.value.push(guitarra);
        }

    }

    const decrementarCantidad = (id) => {
        const index = carrito.value.findIndex(producto => producto.id === id)
        if(carrito.value[index].cantidad <= 1) return
        carrito.value[index].cantidad--

    }

    const incrementarCantidad = (id) => {
        const index = carrito.value.findIndex(producto => producto.id === id)
        if(carrito.value[index].cantidad >= 5) return
        carrito.value[index].cantidad++

    }

    const eliminarProducto = (id) => {
        carrito.value = carrito.value.filter(producto => producto.id !== id)

    }

    const vaciarCarrito = () => {
        carrito.value = []

    }

</script>

<template>

    <Header
        :carrito="carrito"
        :guitarra="guitarra"
        @decrementar-cantidad="decrementarCantidad"
        @incrementar-cantidad="incrementarCantidad"
        @agregar-carrito="agregarCarrito"
        @eliminar-Producto="eliminarProducto"
        @vaciar-carrito="vaciarCarrito"
    />

        <main class="container-xl mt-5">
            <h2 class="text-center">Nuestra Colección</h2>

            <div class="row mt-5">
                <Guitarra
                    v-for="guitarra in guitarras"
                    :key="guitarra.id"
                    :guitarra="guitarra"
                    @agregar-carrito="agregarCarrito"
                />
            </div>
        </main>

    <Footer/>

</template>