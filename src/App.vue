<template>
  <div id="app">
      <swiper :imgWidth="750" :imgList="imgList" :initIndex="0" :loop="true" :auto="false" :autoTime="2000"></swiper>
  </div>
</template>

<script>
import swiper from './swiper.vue'
export default {
  name: 'app',
  data () {
    return {
      imgList:[
          {
             img:'http://img.goodsbag.cn/i/19349_800_800.jpg@750h_750w_1e_1c_0l_100q'
          },
          {
             img:'http://img.goodsbag.cn/i/19350_800_800.jpg@750h_750w_1e_1c_0l_100q'
          },
          {
             img:'http://img.goodsbag.cn/i/19351_800_800.jpg@750h_750w_1e_1c_0l_100q'
          }
      ]
    }
  },
  components:{
      swiper
  }
}
</script>

<style lang="css">
    #app{
        width: 800px;
        margin: 0 auto;
    }
</style>
