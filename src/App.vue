<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods" class="a">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings" class="a">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller" class="a">商家</router-link>
      </div>
    </div>

    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>


  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header';
  import {urlParse} from './common/js/util';
  const ERR_OK = 0;
  export default {
    name: 'app',
    data() {
      return {
        seller: {
            id: (() => {
                let queryParam=urlParse();
                return queryParam.id;
            })()
        }
      }
    },
    created() {
      this.$axios.get('/api/seller?id='+this.seller.id).then((response) => {
        // success
        if (response.data.errno === ERR_OK) {
          // 相当于 extend方法 扩展  es6语法   vue推荐的给对象扩展属性方法
          this.seller=Object.assign({},this.seller,response.data.data);
          //this.seller = response.data.data;
        }
      }, (error) => {
        // error
        console.log(error)
      });
    },
    components: {
      'v-header': header
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"

  #app
    .tab
      display: flex
      width: 100%
      height: 40px
      line-height 40px
      border-1px(rgba(7, 17, 27, 0.1))
      .tab-item
        flex: 1
        text-align center
        & > .a
          display: block
          font-size: 14px
          color: rgb(77, 85, 93)
          &.active
            color: rgb(240, 20, 20)
</style>
