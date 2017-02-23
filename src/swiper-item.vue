<template>
  <div class="swiper-item" :style="{'-webkit-transform':imgLateX}">
        <img :src="img" />
  </div>
</template>

<script>
export default {
  name: 'swiper-item',
  props:{
      img:{
           type:String,
      },
      imgWidth:{
           type:Number,
           default:100
      }
  },
  data () {
    return {

    }
  },
  computed:{
      imgLateX(){
          return 'translate3d('+(this.imgWidth)+'px,0,0)'
      }
  }
}
</script>

<style lang="css">
   .swiper-item{
       width:750px;
       position: absolute;
       top:0;
       left:0;
       height: 300px;

   }
   .swiper-item img{
       width:100%;
       height: 300px;

   }
</style>
