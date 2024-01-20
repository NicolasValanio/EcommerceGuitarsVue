<script setup>
import {ref, reactive, onMounted } from 'vue'
import {db} from '../../Backend/data.js'
import Guitarra from './components/Guitarra.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

const guitarras = ref([])
const carrito = ref([])
onMounted(() =>{
    guitarras.value = db
})

const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)
    if(existeCarrito >= 0){
        carrito.value[existeCarrito].cantidad++
    } else{
        guitarra.cantidad = 1
        carrito.value.push(guitarra)
    }
}

const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    if( carrito.value[index].cantidad <= 1 ) return
        carrito.value[index].cantidad--
    

}

const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    if( carrito.value[index].cantidad >=5 ) return
    carrito.value[index].cantidad++
}

const eliminarProducto = (id) =>{
  
}

</script>


<template >

    <Header
    :carrito="carrito"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @eliminar-producto="eliminarProducto"
    />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>
        <div class="row mt-5">
            <Guitarra
            v-for="guitarra in guitarras"
            :guitarra="guitarra"
            @agregar-carrito="agregarCarrito"
            />
        </div>
    </main>

    <Footer/>

</template>


<style>

</style>