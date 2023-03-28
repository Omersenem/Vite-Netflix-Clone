<template>
  <div class="min-w-[1200px] relative">
    <div class="flex justify-between mr-6">
      <div class="flex items-center font-semibold text-white text-2xl cursor-pointer">
        {{category}}
      </div>
    </div>
    <Carousel
        ref="carousel"
        v-model="currentSlide"
        :items-to-show="8"
        :items-to-scroll="1"
        :wrap-around="true"
        :transition="500"
        snapAlign="start"
        class="bg-transparent"
    >
      <Slide v-for="slide, index in movies" :key="slide" class="flex items-center object-cover text-white bg-transparent">
      <div class="object-cover h-[100%] hover:brightness-125 cursor-pointer"
           :class="currentSlide !== index ? 'border-4 border-transparent' : 'border-4 border-white', currentSlideObject(slide,index)
"
           @click="$event =>fullScreenVideo(index)"
      >
        <img :src="'/images/'+slide.name+'.png'" class="pointer-events-none z-[-1] h-full">
      </div>

      </Slide>
    </Carousel>

  </div>

</template>

<script setup>
import {ref, toRefs} from "vue";
import 'vue3-carousel/dist/carousel.css'
import {Carousel, Slide, Navigation} from "vue3-carousel";
import {useMovieStore} from '../stores/movie'
import {storeToRefs} from "pinia";
import slide from "vue-material-design-icons/Slide.vue";
const useMovie = useMovieStore();
const {movie, showFullVideo} = storeToRefs(useMovie)

let currentSlide = ref(0)
const props=defineProps({category: String, movies:Array })
const {movies, category} =toRefs(props)

const currentSlideObject = (slide, index)=>{
  if(index === currentSlide.value){
    movie.value =slide;
  }
}
const fullScreenVideo = (index)=>{
  currentSlide.value=index
  setTimeout(()=> showFullVideo.value=true, 500 )

}

</script>

<style scoped>

.carousel__prev,
.carousel__next,
.carousel__prev:hover,
.carousel__next:hover{
  color: white;
}
</style>