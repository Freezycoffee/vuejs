<template>
<!-- Carousel -->
<div class="p-5">
    <swiper
    :auto-height="true"
    :slides-per-view="1"
    :loop="true"
    :autoplay="{
        delay: 2000,
    }"
    :navigation="{
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev'
    }"
    :modules="modules">
        <swiper-slide v-for="product in products" :key="product.id">
            <router-link :to="`product/${product.id}`">
            <div class="relative bg-base-100 h-50 w-full my-5 p-0 shadow-sm">
                <figure>
                    <img
                    :src="product.image"
                    :alt="product.name" />
                </figure>
                <div class="absolute w-full -bottom-5 px-5 bg-gradient-to-b from-black/0 to-black/65">
                    <h2 class="text-3xl text-left text-white font-extrabold">{{product.name}}</h2>
                    <p class="text-lg mb-1 text-left text-white font-bold">{{ product.price }}</p>
                </div>
            </div>
            </router-link>
        </swiper-slide>
    </swiper>


    <div class="w-full p-3">
        <h1 class="text-center font-bold text-lg mb-2">Products by Category</h1>
        <ul class="grid grid-cols-3 gap-5 content-center bg-base-100">
            <li class="text-center p-5 rounded-box shadow-sm"
            v-for="category in categories">
                <router-link :to="`category/${category.name}`">{{category.name}}</router-link>
            </li>    
        </ul>
    </div>

</div>
<!-- End Carousel -->

<!-- Hero content -->


<!-- End hero content -->



</template>

<script>
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import 'swiper/css/pagination';
import 'swiper/css/navigation';
import { Autoplay, Pagination, Navigation } from 'swiper/modules';
import '@/styles.css'

import { supabase } from '@/utils/supabase';



export default {
  name: 'HomeView',
  components: {Swiper, SwiperSlide},
  setup(){
    return {modules: [Autoplay, Pagination, Navigation],}
  },
  data() {
    return {
      products: [],
      categories: [],
      errors: [],
      isLoading: true
    }
  },
  mounted(){
    document.title = "Home - freezyCoffee";
    this.getProducts();

  },
  methods: {
    async getProducts(){
        const { data, error } = await supabase.from('products').select();
        const { data: category } = await supabase.from('category').select();
        this.categories = category;
        this.products = data;
        this.isLoading = false
        if(error){
            this.errors.push(error);
        }
    }
    



    },
  }

</script>

<style>
.swiper {
  width: 100%;
  height: auto;
}

.swiper-slide {
  text-align: center;
  font-size: 18px;
  /* background: #444; */

  /* Center slide text vertically */
  display: flex;
  justify-content: center;
  align-items: center;
}

.swiper-slide img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>