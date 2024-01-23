<script setup>
import {ref, onMounted, watch } from 'vue'
import {db} from '../../Backend/data.js'
import Guitarra from './components/Guitarra.vue/'
import Header from './components/Header.vue/'
import Footer from './components/Footer.vue/'
import {toast} from 'vue3-toastify'
import 'vue3-toastify/dist/index.css'

const guitarras = ref([])
const carrito = ref([])
const guitarra = ref([])

onMounted(() =>{
  guitarras.value = db
  guitarra.value = db[3]

  const carritoStorage = localStorage.getItem('carrito')
  if(carritoStorage){
    carrito.value = JSON.parse(carritoStorage)
  }
})

const NotiAgg=(()=>{
  toast.success('Producto añadido al carrito',{
    autoClose:1500,
    position: toast.POSITION.BOTTOM_RIGHT,
    theme: 'dark'
  })
})
//funcion para notificacion
const NotiError=(()=>{
  toast.error('Alcanzaste el maximo permitido por producto',{
    autoClose:3000,
    position: toast.POSITION.BOTTOM_RIGHT,
    theme: 'dark'
  })
})
  
const NotiBorrarCarrito = () => {
  const resolveWithSomeData = new Promise((resolve, reject) => setTimeout(() => resolve({ message: 'Articulos eliminados' }), 2000))
  toast.promise(
    resolveWithSomeData,
    {
      pending: {
        render() {
          return 'Eliminando articulos'
        },
        // other options
        icon: false,
      },
      success: {
        render(res) {
          return res.data.message
        },
        // other options
        icon: '👌',
      },
      error: {
        render(err) {
          // When the promise reject, data will contains the error
          return ('div', 'Err: ' + err.data.message)
          // return 'Err: ' + err.data.message;
        },
        // render: 'just text',
        // render: h('div', 'error'),
      },
    },
    {
      position: toast.POSITION.BOTTOM_RIGHT,
      theme: 'dark'
    }
  )
}
// revisa constantemente los cambios que haya en la pagina y los guarda en el localstorage
watch(carrito, ()=>{
  guardarLocalStorage()
},{
  deep: true
})

// fnc que guarda el carrito atravez del JSON 
const guardarLocalStorage =() =>{
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

const agregarCarrito = (guitarra) => {
  if (guitarra.cantidad >=5) {
    NotiError()
    console.log(guitarra.cantidad)
    return
  }
  const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)
  if(existeCarrito >= 0){
    carrito.value[existeCarrito].cantidad++
    NotiAgg()
  

  } else{
    guitarra.cantidad = 1
    carrito.value.push(guitarra)
    NotiAgg()

    
  }
}

const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  if( carrito.value[index].cantidad <= 1 ) return
  carrito.value[index].cantidad--

}

const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  if( carrito.value[index].cantidad >=5 ){
    // Notifica error y se sale
    NotiError()
    return
  }else { 
    carrito.value[index].cantidad++
  }
}

const eliminarProducto = (id) =>{
  // crear un nuevo array sin el producto que tiene el id proporcionado
  carrito.value = carrito.value.filter(producto => producto.id !== id)
}

const eliminarProductos = () => {
  carrito.value.forEach(producto => {
    producto.cantidad = 0 // Reiniciar la cantidad a cero
  })
  carrito.value = [] // Vaciar el carrito
  NotiBorrarCarrito()
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
            @noti="NotificacionAgg"
            />
        </div>
    </main>

    <Footer/>

</template>


<style>

</style>