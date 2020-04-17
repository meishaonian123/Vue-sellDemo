<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
         <img :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{ seller.name }}</span>
        </div>
        <div class="description">
            {{ seller.description + ' / ' + seller.deliveryTime +'分钟送达'}}
        </div>
        <div v-if="seller.supports" class="supports">
            <div class="support_desc">
              <span class="icon" :class="iconClassMap[seller.supports[0].type]"></span>
              <span class="text">{{ seller.supports[0].description }}</span>
            </div>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
          <span class="count">{{ seller.supports.length }}个</span>
          <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bullentin_wrapper">
      <span class="bullentin_title"></span><span class="bullentin_text">{{ seller.bulletin }}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <div class="detail" v-if="detailShow">
      <div class="detail-wrapper ">
          <div class="detail-main">
            <h1 class="name">{{ seller.name }}</h1>
            <Star :size="24" :score="seller.score"></Star>
            <div class="title">
                <div class="line"></div>
                <div class="text">优惠信息</div>
                <div class="line"></div>
            </div>
            <ul class="supports" v-if="seller.supports">
              <li class="supports-item" v-for="(item,index) in seller.supports" :key="index">
                <span class="icon" :class="iconClassMap[item.type]"></span>
                <span class="text">{{ item.description }}</span>
              </li>
            </ul>
            <div class="title">
                <div class="line"></div>
                <div class="text">商家公告</div>
                <div class="line"></div>
            </div>
            <div class="bullentin">{{ seller.bulletin }}</div>
          </div>
          <div class="detail-close" @click="closeDetail">
           <i class="icon-close"></i>
          </div>
      </div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
import Star from '../star/start.vue';
export default {
  data () {
    return {
        detailShow: false
    };
  },
  methods: {
    showDetail() {
      this.detailShow = true;
    },
    closeDetail() {
      this.detailShow = false;
    }
  },
  created() {
   this.iconClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  components: {
    Star
  },
  props: ['seller']
};
</script>
<style lang="less">
@import '../../common/less/mixin.less';
.header {
  background: rgba(7,17,27,0.5);
  color: #fff;
  position: relative;
  overflow: hidden;
  .content-wrapper {
    position: relative;
    display: flex;
    padding: 24px 12px 18px 24px;
   .avatar{
    img {
    width: 64px;
    height: 64px;
    border-radius: 2px;
       }
   }
   .content {
     margin-left: 16px;
    .title {
      margin: 2px 0 8px 0;
      .brand {
      display: inline-block;
      width: 30px;
      height: 18px;
      // background-color: black;
      .bg-image(brand);
      background-size: 30px 18px;
      vertical-align: bottom;
      }
      .name {
        font-size: 16px;
        font-weight: bold;
        line-height: 18px;
        color: rgb(255,255,255);
        margin-left: 6px;
      }
    }
    .description {
      font-size: 12px;
      line-height: 12px;
      font-weight: 200;
      color: rgb(255,255,255);
      margin-bottom: 5px;
    }
    .supports {
      .icon {
        display: inline-block;
        width: 12px;
        height: 12px;
        // background: blue;
        background-size: 12px 12px;
        background-repeat: no-repeat;
        vertical-align: bottom;
        &.decrease {
          .bg-image(decrease_1);
        }
        &.discount {
          .bg-image(discount_1);
          // background: url('./img/decrease_1@2x.png');
        }
        &.guarantee {
          // background: url('./img/guarantee_1@2x.png');
           .bg-image(guarantee_1);
        }
        &.invoice {
          //  background: url('./img/invoice_1@2x.png');
          .bg-image(invoice_1);
        }
        &.special {
          //  background: url('./img/special_1@2x.png');
          .bg-image(special_1);
        }
      }
      .text {
        font-size: 10px;
        font-weight: 200;
        line-height: 12px;
        color: rgb(255, 255, 255);
      }
    }
  }
  .support-count{
    position: absolute;
    right: 12px;
    bottom: 18px;
    padding: 0 8px;
    border-radius: 14px;
    line-height: 24px;
    color: rgb(255, 255, 255);
    background-color: rgba(0,0,0,0.2);
    // font-size: 20px;
    text-align: center;
    .count{
        font-size: 10px;
        vertical-align: top;
    }
    .icon-keyboard_arrow_right{
         font-size: 10px;
         line-height: 24px;
    }
  }
}
  .bullentin_wrapper {
    height: 28px;
    line-height: 28px;
    background-color: rgba(7,17,27,0.2);
    padding: 0 22px 0 12px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    position: relative;
    .bullentin_title{
      display: inline-block;
      width: 22px;
      height: 12px;
      .bg-image(bulletin);
      background-size: 22px 12px;
      background-repeat: no-repeat;
      vertical-align: top;
      margin-top: 10px;
    }
    .bullentin_text{
      font-size: 10px;
      color: rgb(255, 255,255);
      margin: 0 4px;
    }
    .icon-keyboard_arrow_right{
      position: absolute;
      font-size: 10px;
      right: 12px;
      top: 11px;
    }
 }
  .background {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    right: 0;
    z-index: -1;
    background-color: rgba(7,17,27,0.5);
    filter: blur(10px);
  }
  .detail{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(7, 17, 27, 0.8);
    overflow: auto;
    z-index: 100;
    .detail-wrapper{
      display: flex;
      flex-direction: column;
      min-height: 100%;
        .detail-main{
          flex: 1;
          margin-top: 64px;
          // padding-bottom: 64px;
          .name{
            text-align: center;
            font-size: 16px;
            line-height: 16px;
            font-weight: 700;
            color: rgb(255, 255, 255);
          }
          .title{
              width: 80%;
              display: flex;
              margin: 28px auto 24px auto;
            .line{
              flex: 1;
              // display: inline-block;
              height: 1px;
              background-color: rgba(255,255,255,.8);
              margin: auto;
            }
            .text{
              margin: 0 12px;
              font-size: 14px;
              font-weight: 700;
            }
          }
          .supports {
             width: 80%;
             margin: 0 auto;
             font-size: 0;
             .supports-item{
                margin: 0 0 12px 12px;
                &:last-child{
                  margin-bottom: 0;
                }
              .icon {
                display: inline-block;
                width: 16px;
                height: 16px;
                // background: blue;
                background-size: 16px 16px;
                background-repeat: no-repeat;
                vertical-align: bottom;
                &.decrease {
                  .bg-image(decrease_1);
                }
                &.discount {
                  .bg-image(discount_1);
                  // background: url('./img/decrease_1@2x.png');
                }
                &.guarantee {
                  // background: url('./img/guarantee_1@2x.png');
                  .bg-image(guarantee_1);
                }
                &.invoice {
                  //  background: url('./img/invoice_1@2x.png');
                  .bg-image(invoice_1);
                }
                &.special {
                  //  background: url('./img/special_1@2x.png');
                  .bg-image(special_1);
                }
              }
              .text {
                font-size: 10px;
                font-weight: 200;
                line-height: 12px;
                margin-left: 6px;
                color: rgb(255, 255, 255);
              }
             }
            }
          }
          .bullentin{
            width: 80%;
            margin: auto;
            font-size: 12px;
            color: rgb(255, 255, 255);
            font-weight: 200;
            line-height: 24px;
          }
        .detail-close{
          flex: 0;
          width: 32px;
          height: 32px;
          font-size: 32px;
          color: rgba(255,255,255,.8);
          width: 100%;
          text-align: center;
          .icon-close{
            display: inline-block;
            margin-bottom: 32px;
          }
        }
      }
  }
}
</style>
