<template>
    <div class="shopCar">
        <div class="content" @click="listToggle">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" :class="{'active':totalPrice}">
                        <i class="icon-shopping_cart"></i>
                    </div>
                    <div class="badge" v-show="totalCount">
                        {{totalCount}}
                    </div>
                </div>
                <div class="price" :class="{'active':totalPrice}">
                    ￥{{totalPrice}}
                </div><div class="dec">
                    另需配送费￥{{seller.deliveryPrice}}元
                </div>
            </div>
            <div class="content-right" :class="{'enough': totalPrice>=seller.minPrice}">
                {{payDesc}}
            </div>
        </div>
        <transition name="transHeight">
             <div class="shopCar-list" v-show="shopCarlistShow">
                <div class="list-header">
                    <span class="title">购物车</span>
                    <span class="empty" @click="emptyCar">清空</span>
                </div>
                <div class="list-content" ref="shopCarList">
                    <ul>
                        <li v-for="(food, index) in selectFoods" :key="index" class="food">
                            <span class="name">{{food.name}}</span>
                            <div class="price">
                                ￥{{food.price*food.count}}
                            </div>
                            <CarControl :food="food" class="CarControl"></CarControl>
                        </li>
                    </ul>
                </div>
             </div>
        </transition>
    </div>
</template>
<script type="text/ecmascript-6">
import CarControl from '../carControl/carControl.vue';
import BScroll from 'better-scroll';
export default {
  data () {
    return {
        // count: 1,
        // price: 10,
        // selectFoods: [{count: 1,
        //     price: 10
        // }]
        shopCarlistShow: false
    };
  },
  created() {
  },
  props: ['seller', 'selectFoods'],
  computed: {
      totalPrice() {
        let price = 0;
        this.selectFoods.forEach((food) => {
            if (food.count > 0) {
                price += food.price * food.count;
            }
        });
        return price;
      },
      totalCount() {
          let count = 0;
          this.selectFoods.forEach((food) => {
              if (food.count > 0) {
                    count += food.count;
              }
          });
          return count;
      },
      payDesc() {
        let diff = this.seller.minPrice - this.totalPrice;
        if (!this.totalPrice) {
            return `￥${this.seller.minPrice}起送`;
        } else if (diff > 0) {
            return `还差￥${diff}元起送`;
        } else {
            return '去结算';
        }
      }
  },
  components: {
      CarControl
  },
  methods: {
      listToggle() {
          if (!this.selectFoods.length) {
              return;
          }
          this.shopCarlistShow = !this.shopCarlistShow;
          if (this.shopCarlistShow) {
              this.$nextTick(() => {
                    if (!this.foodScroll) {
                        this._initScroll();
                        } else {
                            this.foodScroll.refresh();
                        }
                });
            }
      },
      _initScroll() {
        this.foodScroll = new BScroll(this.$refs.shopCarList, {
            click: true
            // probeType: 3
        });
        // console.log('_initScroll');
      },
      emptyCar() {
          this.selectFoods.forEach((food) => {
              food.count = 0;
              this.shopCarlistShow = false;
          });
      }
  }
};
</script>
<style lang="less">
    .shopCar{
        width: 100%;
        height: 48px;
        background-color: #141d27;
        position: fixed;
        left: 0;
        bottom: 0;
        z-index: 10;
        // box-sizing: border-box;
        .content{
            display: flex;
            height: 48px;
            .content-left{
                flex: 1;
                height: 48px;
                background-color: #141d27;
                .logo-wrapper{
                    width: 56px;
                    height: 56px;
                    border-radius: 50%;
                    position: relative;
                    top: -12px;
                    left: 18px;
                    line-height: 56px;
                    padding: 6px;
                    box-sizing: border-box;
                    display: inline-block;
                    vertical-align: top;
                    background-color: #141d27;
                    .logo{
                    width: 44px;
                    height: 44px;
                    border-radius: 50%;
                    background-color: #2b343c;
                    font-size: 24px;
                    color: rgba(255, 255, 255, 0.4);
                    text-align: center;
                    line-height: 44px;
                    &.active{
                        background-color: rgb(0, 160, 220);
                        color: white;
                    }
                    }
                    .badge{
                        width: 24px;
                        height: 16px;
                        line-height: 16px;
                        position: absolute;
                        top: 0;
                        right: 0;
                        background-color: red;
                        font-size: 9px;
                        font-weight: 700;
                        color: #fff;
                        line-height: 16px;
                        text-align: center;
                        border-radius: 6px;
                    }
                }
                .price{
                    display: inline-block;
                    color: rgba(255, 255, 255, 0.4);
                    font-size: 16px;
                    height: 48px;
                    line-height: 24px;
                    font-weight: 700;
                    margin: 12px 0px 12px 12px;
                    border-right: 1px solid rgba(255, 255, 255, 0.1);
                    padding-right: 12px;
                    background-color: #141d27;
                    &.active{
                        color: rgb(255, 255, 255);
                    }
                }
                .dec{
                    display: inline-block;
                    color: rgba(255, 255, 255, 0.4);
                    font-size: 16px;
                    height: 48px;
                    line-height: 24px;
                    font-weight: 700;
                    padding-left: 12px;
                }
            }
            .content-right{
                flex: 0 0 105px;
                width: 113px;
                height: 48px;
                color: rgba(255, 255, 255, 0.4);
                font-size: 12px;
                line-height: 48px;
                font-weight: 700;
                text-align: center;
                background-color: #2b343c;
                &.enough{
                    background-color: green;
                    color: white;
                }
            }
        }
        .shopCar-list {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            // height: 340px;
            background-color: #fff;
            z-index: -1;
            transform: translate3d(0,-100%,0);
            &.transHeight-enter{
                transform: translate3d(0,0,0);
            }
            &.transHeight-enter-to{
                transform: translate3d(0,-100%,0);
            }
            &.transHeight-leave{
                transform: translate3d(0,-100%,0);
            }
            &.transHeight-leave-to{
                transform: translate3d(0,0,0);
            }
            &.transHeight-enter-active,&.transHeight-leave-active{
                transition: all 0.6s;
            }
            .list-header{
                height: 40px;
                background-color: #f3f5f7;
                display: flex;
                justify-content: space-between;
                line-height: 40px;
                padding: 0 18px;
                border-bottom: 1px solid #eee;
                .title{
                    font-size: 14px;
                    font-weight: 700;
                    color: rgb(7, 17, 27);
                }
                .empty{
                    font-size: 12px;
                    color: rgb(0, 160, 220);
                }
            }
            .list-content{
                max-height: 217px;
                overflow: hidden;
                .food{
                  height: 48px;
                  padding: 12px 18px;
                  box-sizing: border-box;
                  display: flex;
                  border-bottom: 1px solid #eee;
                  .name{
                      flex: 1;
                      font-size: 14px;
                      line-height: 24px;
                      color: rgb(7, 17, 27);
                  }
                  .price{
                      font-size: 14px;
                      color: rgb(240, 20, 20);
                      line-height: 24px;
                      font-weight: normal;
                      margin: 0 12px 0 18px;
                  }
                }
            }
        }
    }
</style>
