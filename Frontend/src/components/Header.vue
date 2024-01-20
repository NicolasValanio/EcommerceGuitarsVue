<script setup>
const props = defineProps({
    carrito:{
        type: Array,
        required: true
    }
})

defineEmits(['decrementar-cantidad', 'incrementar-cantidad', 'eliminar-producto'])
</script>


<template >
 <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="../../public/logo.svg" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div 
                        class="carrito"
                    >
                        <img class="img-fluid" src="https://res.cloudinary.com/dwrupo75d/image/upload/v1705707653/img/carrito_g3e5gn.png" alt="imagen carrito" />

                        <div id="carrito" class="bg-white p-3">
                            <p v-if="carrito.length === 0" class="text-center">
                                El carrito esta vacio</p>
                            <table v-if="carrito.length > 0" class="w-100 table">
                                <thead>
                                    <tr>
                                        <th>Imagen</th>
                                        <th>Nombre</th>
                                        <th>Precio</th>
                                        <th>Cantidad</th>
                                        <th></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr
                                        v-for="producto in carrito"
                                    >
                                        <td>
                                            <img class="img-fluid" :src="producto.imagen" :alt="'imagen guitarra' + producto.nombre">
                                        </td>
                                        <td>{{producto.nombre}}</td>
                                        <td class="fw-bold">
                                                ${{producto.precio}}
                                        </td>
                                        <td class="flex align-items-start gap-4">
                                            <button
                                                type="button"
                                                class="btn btn-dark"
                                                @click="$emit('decrementar-cantidad', producto.id)"
                                            >
                                                -
                                            </button>
                                                {{producto.cantidad}}
                                            <button
                                                type="button"
                                                class="btn btn-dark"
                                                @click="$emit('incrementar-cantidad', producto.id)"

                                                >
                                                +
                                            </button>
                                        </td>
                                        <td>
                                            <button
                                                class="btn btn-danger"
                                                type="button"
                                                @click="$emit('eliminar-producto', producto.id)"
                                            >
                                                X
                                            </button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>

                            <p v-if="carrito.length > 0" class="text-end">Total pagar: <span class="fw-bold">$899</span></p>
                            <button class="btn btn-dark w-100 mt-3 p-2">Vaciar Carrito</button>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Modelo VAI</h1>
                    <p class="mt-5 fs-5 text-white">Lorem ipsum dolor sit amet consectetur adipisicing elit. Temporibus, possimus quibusdam dolor nemo velit quo, fuga omnis, iure molestias optio tempore sint at ipsa dolorum odio exercitationem eos inventore odit.</p>
                    <p class="text-primary fs-1 fw-black">$399</p>
                    <button 
                        type="button"
                        class="btn fs-4 bg-primary text-white py-2 px-5"
                    >Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="https://res.cloudinary.com/dwrupo75d/image/upload/v1705707655/img/header_guitarra_aegnkf.png" alt="imagen header">
    </header>
</template>