<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
         <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script>
import { urlParse } from './common/js/util';
import header from '@/components/header/header';

const ERR_OK=0;
const debug = process.env.NODE_ENV !== 'production';

export default {
  data() {
    return {
      seller:{
        // id:(()=>{
        //   let queryParam = urlParse();
        //   return queryParam.id;
        //   console.log(queryParam.id);
        // })
      }
    }
  },
  created(){
    this.$http.get('/api/seller').then((resp)=>{
      resp = resp.body;
      console.log(resp);
      if(resp.errno === ERR_OK){
        this.seller = resp.data;
        // this.seller = Object.assign({},this.seller,resp);
        // this.seller=JSON.parse(resp);
         console.log(this.seller);

      }
    })
  },
  components:{
    'v-header':header
  }
}
</script>

<style>

.tab{
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
  border-bottom: 1px solid rgba(7,17,27,0.1);
}
.tab-item{
  flex: 1;
  text-align: center;
}
.tab-item a{
  display: block;
  font-size: 14px;
  color: rgb(77,85,93);
  text-decoration: none;
}
.tab-item > a:active {
    color: rgb(240, 20, 20);
}

</style>
