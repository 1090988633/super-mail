<template>
  <div class="goods">
      <GoodListItem v-for="item in goods"
      :goodsitem="item"/>
     
  </div>
</template>

<script>

import GoodListItem from "./GoodsListItem"
export default {
    components:{
        GoodListItem
    },
   
    props:{
        goods:{
            type:Array,
            default(){
                return []
            }
        }
    }
}
</script>

<style>
    .goods{
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;
        padding: 2px;
    }
</style>