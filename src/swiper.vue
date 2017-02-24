<template>
  <div id="swiper" @mouseover="stopAuto" @mouseout="autoPlay">
      <div class="swiper-box" :style="{width:allCount,'-webkit-transition':transitionConfig,'-webkit-transform':slateX}" ref='swiper'>
          <swiper-item :img="imgList[imgList.length-1].img" :imgWidth="-imgWidth" v-if="loop"/>
          <swiper-item v-for="(item,index) in imgList" :img="item.img" :imgWidth="imgWidth*index"/>
          <swiper-item :img="imgList[0].img" :imgWidth="imgWidth*imgList.length" v-if="loop"/>
      </div>
      <span class='swiper-left' @click="toLeft">←</span>
      <span class='swiper-right' @click='toRight'>→</span>
      <div class="swiper-dots" v-if="dots">
           <button v-for="(item,index) in imgList.length" :class="{active:index==dotsIndex}" @click="toDots(index)">{{item}}</button>
      </div>
  </div>
</template>

<script>
import swiperItem from './swiper-item.vue'
export default {
  name: 'swiper',
  props:{
      imgWidth:{
           type:Number,
           default:100
      },
      imgList:{
           type:Array
      },
      dots:{
           type:Boolean,
           default:true
      },
      arrow:{
           type:Boolean,
           default:true
      },
      initIndex:{
           type:Number,
           default:0
      },
      loop:{
           type:Boolean,
           default:true
      },
      duration:{
           type:Number,
           default:0.3
      },
      auto:{
           type:Boolean,
           default:false
      },
      autoTime:{
           type:Number,
           default:3000
      }
  },
  data () {
    return {
       imgIndex:0,
       durationTime:0.2,
       dotsIndex:0,
       autoer:null,
    }
  },
  components:{
      'swiper-item':swiperItem
  },
  computed:{
      allCount(){
        return (this.imgList.length*this.imgWidth)+'px';
      },
      slateX(){
         console.log('translate3d('+(-this.imgIndex*this.imgWidth)+'px,0,0)')
         return 'translate3d('+(-this.imgIndex*this.imgWidth)+'px,0,0)'
      },
      transitionConfig(){
         return 'all '+this.durationTime+'s';
      },
  },
  methods:{
      toLeft(){
          if(this.loop){
             this.imgIndex--;
             this.dotsIndex--;
             if(this.dotsIndex<=-1) this.dotsIndex=this.imgList.length-1;

             if(this.imgIndex<=-2) this.loopFn('left');

          }
          else {
             if(this.imgIndex==0) return this.dotsIndex=this.imgIndex=this.imgList.length-1;
             this.imgIndex--;
             this.dotsIndex--;

          }

      },
      toRight(){
          if(this.loop){
            // alert(this.loop)

                this.imgIndex++;
                this.dotsIndex++;
                if(this.dotsIndex==this.imgList.length) this.dotsIndex=0;
                if(this.imgIndex==this.imgList.length+1) this.loopFn('right');
          }
          else {
             this.imgIndex++;
             this.dotsIndex++;
             if(this.imgIndex>this.imgList.length-1) return this.dotsIndex=this.imgIndex=0;
          }
      },
      loopFn(type){
          const dur=this.durationTime;
          this.durationTime=0;

          this.imgIndex=type=='right'?0:this.imgList.length-1;

          setTimeout(()=>{
            this.$nextTick(function(){
                 this.durationTime=dur;

                 if(type=='right') this.imgIndex++;
                 else this.imgIndex--;
            })
          },30)
        },
      toDots(index){
          this.dotsIndex=this.imgIndex=index;
      },
      autoPlay(){
           if(this.auto){
               clearInterval(this.autoer);
               this.autoer=setInterval(()=>{
                     this.toRight();
               },this.autoTime)
           }

      },
      stopAuto(){
          if(this.auto) return clearInterval(this.autoer);
      }
  },
  created(){
      this.imgIndex=this.dotsIndex=this.initIndex;

      this.durationTime=this.duration;

      if(this.auto) this.autoPlay();
  },

}
</script>

<style lang="css">
      #swiper{
          position: relative;
          overflow: hidden;
          width: 750px;
          height: 300px;
      }
      .swiper-box{
          /*height: 300px;*/
      }
      .swiper-left{
          position: absolute;
          top:50%;
          display: inline-block;
          transform: translateY(-50%);
          left:0;
          background: rgba(0, 0, 0, 0.5);
          padding: 30px 20px;
          color:#fff;
          font-size: 20px;
          cursor: pointer;
      }
      .swiper-right{
          position: absolute;
          top:50%;
          display: inline-block;
          transform: translateY(-50%);
          right:0;
          background: rgba(0, 0, 0, 0.5);
          padding: 30px 20px;
          color:#fff;
          font-size: 20px;
          cursor: pointer;

      }
      .swiper-dots{
          position: absolute;
          bottom:20px;
          text-align: center;
          left:50%;
          transform: translateX(-50%);
      }
      .swiper-dots button{
          width: 20px;
          height: 20px;
          background: rgba(255, 0, 0, 0.5);
          display: inline-block;
          margin: 0 10px;
          border-radius: 50%;
          color:#fff;
          font-weight: normal;
          border: none;
          outline: none;
          transition: all .2s;
          cursor: pointer;
      }
      .swiper-dots button:hover,.swiper-dots button.active{
          background: rgba(255, 255, 255, 0.5);
          color:#ff0000;
      }
</style>
