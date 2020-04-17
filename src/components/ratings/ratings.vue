<template>
  <div class="rating-header">
    <transition name="transitionDetail">
      <div class="detailWrapper" ref="detailWrapper" v-show="showflg">
        <div class="foodDtail">
          <div class="rating-info">
              <div class="rating-mark">
                  <div class="num">{{seller.score}}</div>
                  <div class="text">综合评分</div>
                  <div class="ratings-desc">高于周边商家{{seller.rankRate}}</div>
              </div>
              <div class="sever-star">
                  <div class="serverScore">
                    <span class="text">服务态度</span>
                    <star :size="36" :score="seller.serviceScore"></star>
                    <span class="num">{{seller.serviceScore}}</span>
                  </div>
                  <div class="foodScore">
                    <span class="text">服务态度</span>
                    <star :size="36" :score="seller.foodScore"></star>
                    <span class="num">{{seller.foodScore}}</span>
                  </div>
                  <div class="deliveryTime">
                    <span class="text">送达时间</span>
                    <span class="time"><span class="timefonst">{{seller.deliveryTime}}分钟</span></span>
                   </div>
              </div>
          </div>
          <div class="driver"></div>
          <div class="foodrating">
              <div class="rating-header">
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
                              <div class="time">{{item.rateTime | time}}</div>
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
  </div>
</template>
<script type="text/ecmascript-6">
import BScroll from 'better-scroll';
import Vue from 'vue';
import star from '../star/start.vue';
import '../../filter/time.js';
const ERR_OK = 0;
export default {
  data() {
    return {
      showflg: true,
         classifyArr: [
            {
                name: '全部',
                // count: this.ratings.length,
                // count: 5,
                active: true
            },
            {
                name: '满意',
                // count: this.ratings.filter((data) => data.score === 5).length,
                // count: 4,
                active: false
            },
            {
                name: '不满意',
                // count: this.ratings.filter((data) => data.score < 5).length,
                // count: 5,
                active: false
            }
        ],
        ratings: [],
        seller: {},
        onlyContent: false
    };
  },
   created() {
    this.$http.get('/api/ratings').then((res) => {
        res = res.body;
        if (res.errno === ERR_OK) {
          this.ratings = res.data;
        //   console.log(this.ratings.length);
          this.classifyArr = [
            {
                name: '全部',
                count: this.ratings.length,
                // count: 5,
                active: true
            },
            {
                name: '满意',
                count: this.ratings.filter((data) => data.score === 5).length,
                // count: 4,
                active: false
            },
            {
                name: '不满意',
                count: this.ratings.filter((data) => data.score < 5).length,
                // count: 5,
                active: false
            }
        ];
          }
    });
    this.$http.get('/api/seller').then((res) => {
        res = res.body;
        if (res.errno === ERR_OK) {
            this.seller = res.data;
        }
    });
     this.$nextTick(() => {
        if (!this.detailWrapper) {
            this._initScroll();
            } else {
                this.detailWrapper.refresh();
            }
    });
  },
  methods: {
       _initScroll() {
       if (!this.detailWrapper) {
               this.detailWrapper = new BScroll(this.$refs.detailWrapper, {
                    click: true
                });
                // console.log(0);
           } else {
               this.detailWrapper.refresh();
            //    console.log(1);
           }
      },
      currentItem(item) {
          this.classifyArr.forEach((data) => {
              data.active = false;
          });
          item.active = true;
      }
  },
  components: {
      star
  },
  computed: {
      evelArr() {
        let selectedIndex = 0;
        this.classifyArr.forEach((data, index) => {
            if (data.active) {
                selectedIndex = index;
            }
        });
        if (this.detailWrapper) {
                this.$nextTick(() => {
                this.detailWrapper.refresh();
            });
        }
        return selectedIndex ? this.ratings.filter((data) => this.onlyContent
            ? data.rateType === selectedIndex - 1 && data.text : data.rateType === selectedIndex - 1)
            : this.ratings.filter((data) => this.onlyContent ? data.text : true);
      }
  }
};
</script>
<style lang="less">
.detailWrapper{
    position: fixed;
    top: 176px;
    left: 0;
    bottom: 0;
    width: 100%;
    z-index: 0;
    background-color: white;
    overflow: hidden;
    // font-size: 0;
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
        .rating-info{
            display: flex;
            padding: 18px 0;
            .rating-mark{
                width: 40%;
                display: flex;
                flex-direction: column;
                text-align: center;
                border-right: 1px solid #eee;
                .num{
                    font-size: 24px;
                    color: rgb(255, 153, 0);
                    line-height: 28px;
                    margin-bottom: 6px;
                }
                .text{
                    font-size: 12px;
                    color: rgb(7, 17, 27);
                    line-height: 12px;
                    margin-bottom: 8px;
                }
                .ratings-desc{
                    font-size: 10px;
                    color: rgb(7, 17, 27);
                    // color: red;
                    line-height: 10px;
                    margin-bottom: 6px;
                }
            }
            .sever-star{
                // flex: 1;
                width: 60%;
                display: flex;
                flex-direction: column;
                font-size: 12px;
                // line-height: 18px;
                color: rgb(7, 17, 27);
                padding: 0 24px;
                .serverScore{
                    display: flex;
                    margin-bottom: 8px;
                    .text{
                        color: rgb(7, 17, 27);
                    }
                }
                .foodScore{
                    display: flex;
                    margin-bottom: 8px;
                }
                .deliveryTime{
                    display: flex;
                    margin-bottom: 8px;
                    .timefonst{
                        color: rgb(147, 153, 159);
                        margin-left: 12px;
                    }
                }
            }
        }
        .driver{
            height: 16px;
            width: 100%;
            background-color: #f3f5f7;
        }
        .foodrating{
            .rating-header{
                padding: 18px 18px 0 18px;
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
