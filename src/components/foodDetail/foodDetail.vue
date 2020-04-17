<template>
<transition name="transitionDetail">
    <div class="detailWrapper" ref="detailWrapper" v-show="showflg">
      <div class="foodDtail">
        <div class="back" @click="showTiggle">
            <i class="icon-arrow_lift"></i>
        </div>
        <div class="food-img">
            <img :src="food.image" width="100%" height="374px" alt="">
        </div>
        <div class="info">
            <h1 class="title">{{food.name}}</h1>
            <div class="desc">
                <span class="sell">月销售:{{food.sellCount}}</span><span class="rating">好评率:{{food.rating}}%</span>
            </div>
            <div class="price">
                <span class="now">￥{{food.price}}</span>
                <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
            </div>
            <div class="addCar">
                <div class="text" v-show="!food.count" @click="addCar($event)">加入购物车</div>
                <carControl :food="food" class="carControl" v-show="food.count"></carControl>
            </div>
        </div>
        <div class="driver"></div>
        <div class="fooddesc">
            <h1 class="title">商品介绍</h1>
            <p class="foodinfo">{{food.info}}</p>
        </div>
        <div class="driver"></div>
        <div class="foodrating">
            <div class="rating-header">
                <h1 class="rating-title">商品评价</h1>
                <div class="rating-state">
                    <span @click="currentItem(item)" class="rating-item" v-for="(item, index) in classifyArr" :key="index"
                    :class="{'active':item.active,'bad': index==2,'badActive':item.active&&index==2}">
                        {{item.name}} <span class="count">{{item.count}}</span>
                    </span>
                </div>
            </div>
            <div class="switch" @click="onlyContent=!onlyContent">
                <i class="icon-check_circle" :class="{'on':onlyContent}"></i>
                <span class="text">只看有内容的评价</span>
            </div>
            <div class="evel-list">
                <ul>
                    <li class="evel-item" v-for="(item,index) in evelArr" :key="index">
                        <div class="userInfo">
                            <div class="time">{{item.rateTime | time }}</div>
                            <div class="user">
                                <span class="name">{{item.username}}</span>
                                <span class="user-avatar">
                                    <img :src="item.avatar" alt="" width="12px" height="12px">
                                </span>
                            </div>
                        </div>
                        <div class="rating-content">
                            <span :class="item.rateType?'icon-thumb_down':'icon-thumb_up'"></span>
                            <span class="text">{{item.text}}</span>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
      </div>
   </div>
</transition>
</template>
<script type="text/ecmascript-6">
import BScroll from 'better-scroll';
import carControl from '../carControl/carControl.vue';
import Vue from 'vue';
import '../../filter/time.js';
const ALL = 0;
export default {
  data () {
    return {
        showflg: false,
        classifyArr: [
            {
                name: '全部',
                count: this.food.ratings.length,
                active: true
            },
            {
                name: '推荐',
                count: this.food.ratings.filter((data) => data.rateType === 0).length,
                active: false
            },
            {
                name: '吐槽',
                count: this.food.ratings.filter((data) => data.rateType).length,
                active: false
            }
        ],
        onlyContent: false
    };
  },
  created() {
    //    this.$nextTick(() => {
    //         this._initScroll();
    //       });
    //  this.classifyArr = [
    //         {
    //             name: '全部',
    //             // count: this.food.ratings.length,
    //             active: true
    //         },
    //         {
    //             name: '推荐',
    //             // count: this.food.ratings.filter((data) => data.rateType === 0).length,
    //             active: false
    //         },
    //         {
    //             name: '吐槽',
    //             // count: this.food.ratings.filter((data) => data.rateType).length,
    //             active: false
    //         }
    //     ];
  },
  props: ['food'],
  methods: {
      showTiggle() {
          this.showflg = !this.showflg;
            this.$nextTick(() => {
            this._initScroll();
          });
      },
       _initScroll() {
           if (!this.detailWrapper) {
               this.detailWrapper = new BScroll(this.$refs.detailWrapper, {
                    click: true
                });
                console.log(0);
           } else {
               this.detailWrapper.refresh();
               console.log(1);
           }
      },
      addCar(event) {
          if (!event._constructed) {
              return;
          }
          Vue.set(this.food, 'count', 1);
        //   console.log('addCar');
      },
      currentItem(item) {
          this.classifyArr.forEach((data) => {
              data.active = false;
          });
          item.active = true;
      }
  },
  components: {
      carControl
  },
  computed: {
      evelArr() {
          let selectIndex = 0;
          console.log(this.food);
          this.classifyArr.forEach((data, index) => {
              if (data.active) {
                  selectIndex = index;
              }
             });
            if (this.detailWrapper) {
                    this.$nextTick(() => {
                    this.detailWrapper.refresh();
                });
            }
        return selectIndex ? this.food.ratings.filter((data) => this.onlyContent
        ? data.rateType === selectIndex - 1 && data.text : data.rateType === selectIndex - 1)
        : this.food.ratings.filter((data) => this.onlyContent ? data.text : true);
        }
  }
};
</script>
<style lang="less">
.detailWrapper{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    bottom: 48px;
    z-index: 0;
    background-color: white;
    transform: translate3d(0, 0, 0);
        &.transitionDetail-enter{
        transform: translate3d(100%,0,0);
    }
    &.transitionDetail-enter-to{
        transform: translate3d(0,0,0);
    }
    &.transitionDetail-leave{
        transform: translate3d(0,0,0);
    }
    &.transitionDetail-leave-to{
        transform: translate3d(100%,0,0);
    }
    &.transitionDetail-enter-active,&.transitionDetail-leave-active{
        transition: all 0.6s;
    }
    .foodDtail{
        .back{
            font-size: 20px;
            position: absolute;
            top: 10px;
            left: 10px;
            color: #00a0dc;
        }
        .info{
            padding: 18px;
            position: relative;
            // background-color: #fff;
            display: flex;
            flex-direction: column;
            .title{
                font-size: 14px;
                line-height: 14px;
                color: rgb(7, 17, 27);
                font-weight: 700;
                margin-bottom: 8px;
            }
            .desc{
                font-size: 10px;
                line-height: 10px;
                color: rgb(147, 153, 159);
                margin-bottom: 9px;
                .sell{
                    margin-right: 6px;
                }
            }
            .price{
                font-size: 14px;
                line-height: 24px;
                font-weight: 700;
                .now{
                    color: rgb(240, 20, 200);
                }
                .old{
                    color: rgb(47, 153, 159);
                    font-size: 10px;
                    font-weight: normal;
                }
            }
            .addCar{
                width: 74px;
                height: 24px;
                line-height: 24px;
                text-align: center;
                position: absolute;
                right: 18px;
                bottom: 36px;
                width: 80px;
                .text{
                    color: white;
                    font-size: 10px;
                    background-color: rgb(0, 160, 220);
                    border-radius: 12px;
                    // display: none;
                }
                // .carControl{
                //     position: absolute;
                //     top: 12px;
                //     right: 12px;
                // }
            }
        }
        .driver{
            height: 16px;
            width: 100%;
            background-color: #f3f5f7;
        }
        .fooddesc{
            padding: 16px;
            .title{
                font-size: 14px;
                line-height: 14px;
                color: rgb(7, 17, 27);
                font-weight: 700;
            }
            .foodinfo{
                font-size: 12px;
                line-height: 24px;
                color: rgb(77, 85, 93);
                font-weight: 200;
                margin: 6px 8px 0 8px;
            }
        }
        .foodrating{
            // height: 500px;
            .rating-header{
                // height: 300px;
                padding: 18px 18px 0 18px;
                .rating-title{
                    font-size: 14px;
                    line-height: 14px;
                    color: rgb(7, 17, 27);
                    font-weight: 700;
                    margin-bottom: 18px;
                }
                .rating-state{
                     padding-bottom: 18px;
                     border-bottom: 1px solid #eee;
                    .rating-item{
                        display: inline-block;
                        background-color: rgba(0,160,220,0.2);
                        padding: 8px 12px;
                        &.active{
                            color: white;
                            background: rgb(0,169,220);
                        }
                        &.bad{
                            background: rgba(77,85,93,0.2);
                        }
                        &.badActive{
                            background: #4d555d;
                        }
                        .count{
                            font-size: 8px;
                        }
                    }
                }
            }
            .switch{
                color: rgb(147, 153, 159);
                padding: 12px 18px;
                border-bottom: 1px solid #eee;
                .icon-check_circle{
                    font-size: 24px;
                    line-height: 24px;
                    vertical-align: bottom;
                    &.on{
                        color: #00c850;
                    }
                }
                .text{
                    font-size: 12px;
                    line-height: 24px;
                }
            }
            .evel-list{
                .evel-item{
                    padding: 13px 18px;
                    border: 1px solid #eee;
                    .userInfo{
                        display: flex;
                        justify-content: space-between;
                        font-size: 10px;
                        color: rgb(147, 153, 159);
                        line-height: 12px;
                        margin-bottom: 6px;
                    }
                    .rating-content{
                        .icon-thumb_down{
                            font-size: 12px;
                            line-height: 20px;
                            color: rgb(147, 153, 159);
                            vertical-align: top;
                        }
                        .icon-thumb_up{
                            font-size: 12px;
                            line-height: 20px;
                            color: rgb(0,169,220);
                            vertical-align: top;
                        }
                        .text{
                            font-size: 12px;
                            line-height: 16px;
                            color: rgb(7, 17, 27);
                        }
                    }
                }
            }
        }
    }
}
</style>
