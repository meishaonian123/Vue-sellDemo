<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <li class="menu-item" v-for="(item,index) in goods" :key="index" @click="menuClick(index,$event)" :class="index == menuCurrentIndex?'current':''">
              <span class="text">
                <!-- <span v-show="item.type>0" class="icon" :class="iconClassMap[item.type]"></span> -->
                <IconMap v-show="item.type>0" :iconType="item.type"></IconMap>
                {{ item.name }}
              </span>
          </li>
        </ul>
    </div>
    <div class="foods-wrapper" ref="foodWrapper">
      <ul>
        <li v-for="(item ,index) in goods" :key="index" class="food-list food-list-hook" >
            <h1 class="title" style="height:26px;">{{item.name}}</h1>
            <ul>
              <li v-for="(item,index) in item.foods" :key="index" class="food-item">
                  <div class="food-icon" @click="showDtail(item)">
                  <!-- <div class="food-icon"> -->
                    <img :src="item.icon" alt="" width="57" height="57">
                  </div>
                  <div class="content">
                    <h2 class="food-name">{{item.name}}</h2>
                    <p class="description" v-show="item.description">{{item.description}}</p>
                    <div class="sell-info">
                      <span class="sellCount">月销售{{item.sellCount}}份</span><span class="rating">好评率{{item.rating}}</span>
                    </div>
                    <div class="price">
                      <span class="nowPrice">￥{{item.price}}</span><span class="oldPrice" v-show="item.oldprice">￥{{item.oldPrice}}</span>
                    </div>
                    <div class="carcontrol-wrapper">
                      <CarControl :food="item"></CarControl>
                    </div>
                  </div>
              </li>
            </ul>
        </li>
      </ul>
    </div>
    <ShopCar :seller="seller" :selectFoods="selectFoods"></ShopCar>
    <FoodDtail :food="selectedFood" ref="FoodDtail" v-if="selectedFood"></FoodDtail>
  </div>
</template>
<script type="text/ecmascript-6">
import IconMap from '../iconMap/iconMap.vue';
import BScroll from 'better-scroll';
import ShopCar from '../shopCar/shopCar.vue';
import CarControl from '../carControl/carControl.vue';
import FoodDtail from '../foodDetail/foodDetail.vue';
const ERR_OK = 0;
export default {
  data () {
    return {
      goods: [],
      iconClassMap: [],
      listHeight: [],
      scrollY: 0,
      // showDetailFlag: false,
      selectedFood: ''
    };
  },
  props: ['seller'],
  created() {
    this.iconClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    this.$http.get('/api/goods').then((res) => {
        res = res.body;
        if (res.errno === ERR_OK) {
          this.goods = res.data;
          }
          // console.log(res);
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          });
    });
  },
  methods: {
    _initScroll() {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      });
      this.foodScroll = new BScroll(this.$refs.foodWrapper, {
        click: true,
        probeType: 3

      });
      this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.round(Math.abs(pos.y));
          // console.log(this.scrollY);
      });
    },
    _calculateHeight() {
      let height = 0;
      this.listHeight.push(height);
      let foodList = this.$refs.foodWrapper.querySelectorAll('.food-list-hook');
      // console.log(foodList);
      for (let i = 0; i < foodList.length; i++) {
        let Item = foodList[i];
        height += Item.clientHeight;
        // console.log(height);
        this.listHeight.push(height);
      }
    },
    menuClick(index, event) {
      console.log(index);
       if (!event._constructed) {
        return;
      }
      this.foodScroll.scrollTo(0, -this.listHeight[index], 300);
    },
    showDtail(item) {
      this.selectedFood = item;
      this.$nextTick(() => {
        this.$refs.FoodDtail.showTiggle();
      });
    }
  },
  computed: {
    menuCurrentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
            let topHeight = this.listHeight[i];
            let bottomHeight = this.listHeight[i + 1];
            if (!bottomHeight || (this.scrollY >= topHeight && this.scrollY < bottomHeight)) {
              // console.log(i);
              return i;
            }
        }
        return 0;
    },
    selectFoods() {
       let foods = [];
       let good = this.goods.forEach((good) => {
         good.foods.forEach((food) => {
              if (food.count > 0) {
                foods.push(food);
              }
         });
       });
       return foods;
    }
  },
  components: {
    IconMap,
    ShopCar,
    CarControl,
    FoodDtail
  }
};
</script>
<style lang="less">
@import '../../common/less/mixin.less';
  .goods{
    display: flex;
    width: 100%;
    position: absolute;
    top: 175px;
    bottom: 46px;
    overflow: hidden;
    .menu-wrapper{
      flex: 0 0 80px;
      width: 80px;
      margin-top: 2px;
      background-color: #eee;
      .menu-item{
        &.current {
          background-color: #fff;
        }
        display: table;
        width: 56px;
        height: 54px;
        padding: 0 12px;
        line-height: 14px;
        vertical-align: top;
        &:hover{
          background-color: #fff;
        }
        }
        .text{
           display: table-cell;
           width: 56px;
           font-size: 12px;
           font-weight: 200;
           color: black;
           vertical-align: middle;
           border-bottom: 1px solid rgba(7, 17, 27, 0.1);
        }
    }
    .foods-wrapper{
      flex: 1;
      margin-top: 2px;
      .food-list{
        .title{
          height: 26px;
          line-height: 26px;
          padding-left: 12px;
          font-size: 12px;
          color: rgb(147,153,159);
          background: #f3f5f7;
          border-left: 2px solid #d9dde1;
        }
        .food-item{
          display: flex;
          margin: 18px;
          position: relative;
          .content{
            margin: 2px 0 0 10px;
            font-size: 10px;
            color: rgb(147, 153, 159);
            line-height: 10px;
            .food-name{
              font-size: 14px;
              line-height: 14px;
              color: rgb(7, 17, 27);
            }
            .description{
              margin-top: 8px;
            }
            .sell-info{
              margin-top: 8px;
              .sellCount{
                margin-right: 12px;
              }
            }
            .price{
              font-size: 14px;
              line-height: 24px;
              font-weight: 700;
              .nowPrice{
                color: red;
              }
              .olgPrice{
                text-decoration: line-through;
                font-size: 10px;
              }
            }
            .carcontrol-wrapper{
              position: absolute;
              right: 0;
              bottom: -16px;
            }
          }
        }
      }
    }
  }
</style>
