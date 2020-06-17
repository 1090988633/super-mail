<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <BS class="content" 
    ref="content" 
    :probe-type="3" 
    @scroll="contentscroll"
    :pullUpLoad="true"
    @pullingUp="pullingUp">
    <home-swiper :banners="banners" />
    <recommend-view :recommends="recommends" />
    <feature />
    <tap-control class="tab-control" :title="['流行','现代','复古']" @tabClick="tabClick" />
    <GoodList :goods="goods[curentType].list" />
      </BS>
      <Backon @click.native="clickon" v-show="isture"/>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar';
  import HomeSwiper from './childComps/HomeSwiper'
  import RecommendView from './childComps/RecommendView';

  import {getHomeMultidata,getHomedata} from "network/home";
 
 import Feature from './childComps/Feature';
  import TapControl from './childComps/TabControl';
  import GoodList from 'components/content/goodslist/GoodList';
  import BS from 'components/common/betterscroll/Bscroll'
  import Backon from 'components/content/backon/backon'
  export default {
    name: "Home",
    components: {
      NavBar,
      HomeSwiper,
      RecommendView,
      Feature,
      TapControl,
      GoodList,
      BS,
      Backon
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods:{
          'pop':{page:0,list:[]},
          'new':{page:0,list:[]},
          'sell':{page:0,list:[]}
        },
        curentType : 'pop',
        isture:false
      }
    },
    created() {
      // 1.请求多个数据
      this.getHomeMultidata();

      this.getHomedata('pop');
      this.getHomedata('new');
      this.getHomedata('sell');
    },
    methods:{
      getHomeMultidata(){
      getHomeMultidata().then(res => {
        // this.result = res;
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });},
      getHomedata(type){
        const page = this.goods[type].page+1;
        getHomedata(type,page).then(res=>{
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page++;
        this.$refs.content.finishPullUp()
        
      })
      },
      tabClick(index){
        switch(index){
          case 0:
            this.curentType = 'pop'
            break
            case 1:
              this.curentType = 'new'
              break
              case 2:
                this.curentType = 'sell'
                break
        }
      },
      clickon(){
        this.$refs.content.scrollTo(0,0)
      },
      contentscroll(position){
        this.isture = (-position.y)>1000
      },
      pullingUp(){
        this.getHomedata(this.curentType)
        
      }
    }
  }
</script>

<style scoped>
#home {
  height: 100vh;
  position: relative;
}

.home-nav {
  background-color: var(--color-tint);
  color: #fff;

  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 9;
}

.tab-control {
  position: sticky;
  top: 44px;
  z-index: 9;
}

.content{
  position: absolute;
  overflow: hidden;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;

}
</style>
