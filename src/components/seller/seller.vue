<template>
<div class="sellerwrapper" ref="sellerwrapper">
  <div class="sellerContent">
    <div class="info">
      <div class="title">
        <div class="text">{{seller.name}}</div>
        <div class="star">
            <star :size="36" :score="seller.score" class="rating-star"></star>({{seller.ratingCount}})
             <span class="sellCount">月销售: {{seller.sellCount}}</span>
        </div>
        <div class="collect" @click="collectflg=!collectflg">
          <i class="icon-favorite" :class="{'active':collectflg}"></i>
          <span class="text">{{collectflg?'已收藏':'未收藏'}}</span>
        </div>
      </div>
      <div class="remark">
        <div class="block">
          <h2>起送价</h2>
          <div class="content">
            <span class="num">{{seller.minPrice}}</span>元
          </div>
        </div>
         <div class="block">
          <h2>商家配送</h2>
          <div class="content">
            <span class="num">{{seller.deliveryPrice}}</span>元
          </div>
        </div>
         <div class="block">
          <h2>平均配送时间</h2>
          <div class="content">
            <span class="num">{{seller.deliveryTime}}</span>元
          </div>
        </div>
      </div>
    </div>
    <div class="driver"></div>
    <div class="activities">
       <h2 class="title">公告与活动</h2>
       <span class="bulletin">{{seller.bulletin}}</span>
    </div>
    <div class="supports">
      <ul>
        <li v-for="(item, index) in seller.supports" :key="index" class="support-item">
            <span class="icon" :class="iconClassMap[seller.supports[index].type]"></span>
            <span class="text">{{ seller.supports[index].description }}</span>
        </li>
      </ul>
    </div>
     <div class="driver"></div>
     <div class="sellerimg" ref="sellerimg">
        <h2 class="text">商家实景</h2>
        <div class="imgWrapper" ref="imgWrapper">
          <div ref="picsWrapper">
            <img v-for="(pic, index) in seller.pics" :key="index" :src="pic" width="120" height="90">
          </div>
        </div>
     </div>
     <div class="driver"></div>
     <div class="seller-info">
        <h2 class="text">商家信息</h2>
        <ul>
          <li v-for="(item, index) in seller.infos" :key="index" class="info-dec">{{item}}</li>
        </ul>
     </div>

  </div>
</div>
</template>
<script type="text/ecmascript-6">
import star from '../star/start.vue';
import BScroll from 'better-scroll';
const ERR_OK = 0;
export default {
  data () {
    return {
      seller: {},
      collectflg: false
    };
  },
  created() {
    this.$http.get('/api/seller').then((res) => {
      res = res.body;
      if (res.errno === ERR_OK) {
        this.seller = res.data;
        // console.log(this.seller);
        this.$nextTick(() => {
          if (!this.sellerwrapper) {
              this._initScroll();
          } else {
            this.sellerwrapper.refresh();
          }
          this._initPicScroll();
        });
      }
    });
    this.iconClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  methods: {
    _initScroll() {
       if (!this.sellerwrapper) {
            this.sellerwrapper = new BScroll(this.$refs.sellerwrapper, {
                click: true
            });
           } else {
               this.sellerwrapper.refresh();
           }
      },
     _initPicScroll() {
        if (this.picsScroll) {
          return;
        }
        const PIC_WIDTH = 120;
        const MARGIN = 6;
        let picLen = this.seller.pics.length;
        this.$refs.picsWrapper.style.width = PIC_WIDTH * picLen + MARGIN * (picLen - 1) + 'px';
        this.picsScroll = new BScroll(this.$refs.imgWrapper, {
          scrollX: true
        });
    }
  },
  components: {
    star
  }
};
</script>
<style lang="less">
@import '../../common/less/mixin.less';
.sellerwrapper{
  width: 100%;
  position: absolute;
  top: 174px;
  bottom: 0;
  left: 0;
  overflow: hidden;
  .sellerContent{
    .driver{
        height: 16px;
        width: 100%;
        background-color: #f3f5f7;
    }
    .info{
      padding: 18px;
      .title{
        font-size: 10px;
        color: rgb(77, 85, 93);
        border-bottom: 1px solid #eee;
        position: relative;
        padding-bottom: 18px;
        .text{
          font-size: 14px;
          line-height: 14px;
          color: rgb(7, 17, 27);
          margin-bottom: 8px;
        }
        .star{
          display: inline-block;
          .rating-star{
            &:first-child{
              margin-left: 3px;
            }
            &:last-child{
              margin-right: -6px;
            }
          }
          .sellCount{
            margin-left: 4px;
          }
        }
        .collect{
          position: absolute;
          width: 50px;
          top: 7px;
          right: 0;
          text-align: center;
          .icon-favorite{
            font-size: 24px;
            line-height: 24px;
            &.active{
              color: rgb(240, 20, 20);
            }
          }
          .text{
            display: block;
            margin-top: 4px;
          }
        }
      }
      .remark{
        display: flex;
        .block{
          text-align: center;
          width: 33.33%;
          margin: 18px 0;
          font-size: 10px;
          line-height: 10px;
          color: rgb(147, 153, 159);
          border-right: 1px solid #eee;
          &:last-child{
            border-right: 0;
          }
          .content{
            margin-top: 4px;
            color: rgb(7, 17, 27);
            font-weight: 200;
            .num{
              font-size: 24px;
              line-height: 24px;
            }
          }
        }
      }
    }
    .activities{
      padding: 18px 12px 16px 12px;
      font-size: 14px;
      border: 1px solid #eee;
      .title{
        line-height: 14px;
        color: rgb(7, 17, 27);
        margin-bottom: 8px;
      }
      .bulletin{
        font-size: 12px;
        line-height: 24px;
        font-weight: 200;
        color: rgb(240, 20, 20);
      }
    }
    .supports{
      .support-item{
        padding: 16px 12px 16px 12px;
        font-size: 0;
        .icon {
          display: inline-block;
          width: 16px;
          height: 16px;
          // line-height: 16px;uy-
          // background: blue;
          background-size: 12px 12px;
          background-repeat: no-repeat;
          vertical-align: top;
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
        .text{
          font-size: 12px;
          line-height: 16px;
          font-weight: 200;
          color: rgb(7, 17, 27);
        }
      }
    }
    .sellerimg{
      padding: 18px 0 18px 18px;
      white-space: nowrap;
      // overflow: hidden;
      .text{
        font-size: 14px;
        line-height: 14px;
        color: rgb(7, 17, 27);
        margin-bottom: 12px;
      }
      .imgWrapper{
        overflow: hidden;
      }
    }
    .seller-info{
      padding: 18px 12px 0 12px;
      .text{
        font-size: 14px;
        line-height: 14px;
        color: rgb(7, 17, 27);
        margin-bottom: 12px;
      }
      .info-dec{
        padding: 16px 12px 16px 12px;
        border-top: 1px solid #eee;
        font-size: 12px;
        line-height: 16px;
        color: rgb(7, 17, 27);
        font-weight: 200;
      }
    }
  }
}
</style>
