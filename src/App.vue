<script setup>
import { ref, reactive, onMounted, watch } from 'vue';
import {db} from './data/guitarras.js';
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';

const guitarras = ref([]);
const carrito = ref([]);
const guitarra = ref({});

watch(carrito,()=>{
    guardarLocalStorage();
},{
    deep:true
});

onMounted(() => {
  guitarras.value = db;
  guitarra.value = db[3];
  const carritoStorage = localStorage.getItem('carrito');
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage);
  }
})

const guardarLocalStorage =()=>{
    localStorage.setItem('carrito',JSON.stringify(carrito.value));
}

const agregarCarrito =(guitarra)=>{
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id);
      if (existeCarrito >=0) {
        carrito.value[existeCarrito].cantidad++;
      }else{
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
      }  
}

const decrementaCantidad =(id)=>{
    const decrementaCarrito = carrito.value.findIndex(producto => producto.id === id);
    if (carrito.value[decrementaCarrito].cantidad > 1) {
        carrito.value[decrementaCarrito].cantidad--;
    }
}

const incrementaCantidad =(id)=>{
    const incrementaCarrito = carrito.value.findIndex(producto => producto.id === id);
    carrito.value[incrementaCarrito].cantidad++;
}

const eliminaCarrito =(id)=>{
    const eliminaCarrito = carrito.value.findIndex(producto => producto.id === id);
    carrito.value.splice(eliminaCarrito,1);
}

const vaciarCarrito =()=>{
    carrito.value = [];
}

</script>

<template>
    <Header 
        :carrito="carrito"
        :guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
        @decrementa-cantidad="decrementaCantidad"
        @incrementa-cantidad="incrementaCantidad"
        @elimina-carrito="eliminaCarrito"
        @vaciar-carrito="vaciarCarrito"
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

    <Footer />

</template>

