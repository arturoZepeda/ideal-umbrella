<script setup>
import { ref, reactive, onMounted } from 'vue';
import {db} from './data/guitarras.js';
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';

/*const state = reactive({
    guitarras:db
})*/
const guitarras = ref(db);
const carrito = ref([]);
onMounted(() => {
  guitarras.value = db;
  //state.guitarras = db;
})
const agregarCarrito =(guitarra)=>{
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
    console.log(carrito.value);
}
</script>

<template>
    <Header />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">  
          <Guitarra 
            v-for="guitarra in guitarras"
            :guitarra="guitarra"
            @agregar-carrito="agregarCarrito"
          />
<!-- INICIO GUITARRA 
        ejemplo de como se puede hacer con un v-for
        <div class="row mt-5">
            <div v-for="guitarra in guitarras" class="col-md-6 col-lg-4 my-4 row align-items-center">
                <div class="col-4">
                    <img class="img-fluid" src="/img/{{ guitarra.imagen }}.jpg" alt="imagen guitarra">
                </div>
                <div class="col-8">
                    <h3 class="text-black fs-4 fw-bold text-uppercase">{{ guitarra.nombre }}</h3>
                    <p>{{ guitarra.descripcion }}</p>
                    <p class="fw-black text-primary fs-3">${{ guitarra.precio }}</p>
                    <button 
                        type="button"
                        class="btn btn-dark w-100 "
                    >Agregar al Carrito</button>
                </div>
            </div>-->
<!-- FIN GUITARRA -->
        </div>
    </main>



</template>

