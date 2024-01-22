<script setup>
import {ref, onMounted, watch } from 'vue'
import {db} from '../../Backend/data.js'
import Guitarra from './components/Guitarra.vue/'
import Header from './components/Header.vue/'
import Footer from './components/Footer.vue/'

const guitarras = ref([])
const carrito = ref([])
const guitarra = ref([])

// revisa constantemente los cambios que haya en la pagina y los guarda en el localstorage
watch(carrito, ()=>{
  guardarLocalStorage()
},{
  deep: true
})

onMounted(() =>{
  guitarras.value = db
  guitarra.value = db[3]

  const carritoStorage = localStorage.getItem('carrito')
  if(carritoStorage){
    carrito.value = JSON.parse(carritoStorage)
  }
})

// fnc que guarda el carrito atravez del JSON 
const guardarLocalStorage =() =>{
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

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
  // crear un nuevo array sin el producto que tiene el id proporcionado
  carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const eliminarProductos= () =>{
  carrito.value = []
}

</script>

<template >

    <Header
    :guitarra="guitarra"
    :carrito="carrito"
    @agregar-carrito="agregarCarrito"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @eliminar-producto="eliminarProducto"
    @eliminar-productos="eliminarProductos"
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