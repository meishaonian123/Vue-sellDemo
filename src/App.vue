<template>
  <div id="app">
      <v-header :seller="seller"></v-header>
      <div class="tab">
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
import Header from './components/header/header.vue';
export default {
  data () {
    return {
      seller: {}
    };
  },
  created() {
    this.$http.get('/api/seller').then((res) => {
        this.seller = res.body.data;
    });
  },
  components: {
    'v-header': Header
    }
};
</script>
<style lang="less">
// @import './common/less/mixin.less';
// @import './common/less/fonts.less';
@import './common/less/index.less';
#app {
   .tab {
    display: flex;
    width: 100%;
    // text-align: center;
    .border-1px(rgb(7,17,27,0.1));
    .tab-item {
      // width: 33.33%;
      flex: 1;
      height: 40px;
      font-size: 14px;
      line-height: 40px;
      color: rgb(77, 85, 93);
      text-align: center;
      .active {
          color:rgb(240, 20, 20);
        }
    }
  }
}
</style>
