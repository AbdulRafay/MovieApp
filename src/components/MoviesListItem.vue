<template>
  <li class="movies-item">
    <!-- <a class="movies-item__link" :class="{'no-image': noImage}" :href="'/movie/' + movie.id" @click.prevent="openMoviePopup(movie.id, true)"> -->
    <!-- <a class="movies-item__link" :class="{'no-image': noImage}" :href="'/movie/' + movie.id"> -->
      <figure class="movies-item__poster">
        <!-- <img v-if="!noImage" class="movies-item__img" src="~assets/placeholder.png" v-img="poster()" alt=""> -->
        <iframe v-if="!noVideo" :id=movie.video_key type="text/html" width="640" height="360" class="movies-item__ogdVideo"
            :src="videoSrc()"
            frameborder="0">
        </iframe>
        <img v-if="noVideo" class="movies-item__img is-loaded" src="~assets/no-image.png" alt="">
      </figure>
      <div class="movies-item__content">
        <p class="movies-item__title">{{ movie.title }}</p>
        <p class="movies-item__title">Rated: {{ movie.rated }}</p>
        <p class="movies-item__title">Released: {{ movie.released }}</p>
        <p class="movies-item__title">Run Time: {{ movie.runtime }}</p>
        <p class="movies-item__title">Year: {{ movie.year }}</p>
         <p class="movies-item__title">Genere: {{ movie.genere }}</p>
      </div>
   <!--  </a> -->
  </li>
</template>

<script>
import img from '../directives/v-image.js'

export default {
  props: ['movie'],
  directives: {
    img: img
  },
  data(){
    return{
      noImage: false,
      noVideo:false
    }
  },
  methods: {
    videoSrc() {
      if(this.movie.video_key){
        return 'https://www.youtube.com/embed/'+this.movie.video_key+'?autoplay=0';
      } else {
        this.noVideo = true;
      }
    },
    poster() {
      if(this.movie.poster_path){
        return 'https://image.tmdb.org/t/p/w370_and_h556_bestv2' + this.movie.poster_path;        
      } else {
        this.noImage = true;
      }
    },
    openMoviePopup(id, event){
      eventHub.$emit('openMoviePopup', id, event);
    }
  }
}
</script>

<style lang="scss">
@import "./src/scss/variables";
@import "./src/scss/media-queries";
.movies-item{
  &__link{
    text-decoration: none;
    color: rgba($c-dark, 0.5);
    font-weight: 300;
  }
  &__content{
    padding-top: 15px;
  }
  &__poster{
    transition: transform 0.5s ease, box-shadow 0.5s ease;
    transform: translateZ(0);
    background: $c-white;
  }
  &__img{
    width: 100%;
    opacity: 0;
    transform: scale(0.97) translateZ(0);
    transition: opacity 0.5s ease, transform 0.5s ease;
    &.is-loaded{
      opacity: 1;
      transform: scale(1);
    }
  }
  &__ogdVideo{
    width: 100%;    
    transform: scale(0.97) translateZ(0);
    transition: opacity 0.5s ease, transform 0.5s ease;
    &.is-loaded{
      opacity: 1;
      transform: scale(1);
    }
  }
  &__link:not(.no-image):hover &__poster{
    transform: scale(1.03);
    box-shadow: 0 0 10px rgba($c-dark, 0.1);
  }
  &__title{
    margin: 0;
    font-size: 11px;
    letter-spacing: 0.5px;
    transition: color 0.5s ease;
    @include mobile-ls-min{
      font-size: 12px;
    }
    @include tablet-min{
      font-size: 14px;
    }
  }
  &__link:hover &__title{
    color: $c-dark;
  }
}
</style>
