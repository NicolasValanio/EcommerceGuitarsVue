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

const agregarcarrito = (guitarra) =>{
    carrito.cantidad = 1;
    carrito.value.push(guitarra)
}
</script>


<template >

    <Header/>

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitarra
            v-for="guitarra in guitarras"
            :guitarra="guitarra"
            @agregar-carrito="agregarcarrito"
            />
        </div>
    </main>

    <Footer/>

</template>


<style>

</style>