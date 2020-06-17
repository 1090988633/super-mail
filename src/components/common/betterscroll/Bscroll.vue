<template>
  <div class="wrapper" ref="wrapper">
      <div class="content">
          <slot/>
      </div>
  </div>
</template>

<script>
import BS from "better-scroll"
export default {
    props:{
        probeType:{
            type:Number,
            default:0
        },
        pullUpLoad:{
            type:Boolean,
            default:false
        }
    },
    data(){
        return {
            scroll:null,
           
        }
    },
    mounted(){
        this.scroll = new BS(this.$refs.wrapper,{
            click:true,
            probeType: this.probeType,
            pullUpLoad: this.pullUpLoad
        })
       this.scroll.on('scroll',(position)=>{
           this.$emit('scroll',position)
       })
       this.scroll.on('pullingUp',()=>{
           this.$emit('pullingUp')
       })
    },
    methods:{
        scrollTo(x,y,time=500) {
            this.scroll.scrollTo(x,y,time)
        },
        finishPullUp(){
            this.scroll.finishPullUp()
        }
    }
}
</script>

<style>

</style>