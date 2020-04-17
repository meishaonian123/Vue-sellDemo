<template>
    <div class="carControl">
        <!-- <div class="car-decrease">
            <span class="icon-remove_circle_outline"></span>
        </div>
        <div class="car-count">12</div>
        <div class="car-add">
            <span class="icon-add_circle"></span>
        </div> -->
        <span class="icon-remove_circle_outline car-decrease" v-show="food.count" @click="decreaseCar($event)"></span>
        <span class="car-count" v-show="food.count">{{food.count}}</span>
        <span class="car-add icon-add_circle" @click="addCar($event)"></span>
    </div>
</template>
<script type="text/ecmascript-6">
import Vue from 'vue';
export default {
  data () {
    return {
    };
  },
  props: ['food'],
  methods: {
      addCar(event) {
          if (!event._constructed) {
              return;
          }
           if (!this.food.count) {
                    Vue.set(this.food, 'count', 1);
                } else {
                    this.food.count++;
                }
          },
        decreaseCar(event) {
             if (!event._constructed || !this.food.count) {
              return;
                }
              this.food.count--;
            }
    }
};
</script>
<style lang="less">
.carControl{
    // font-size: 24px;
    line-height: 24px;
    color: rgb(0, 160, 220);
    text-align: center;
    font-size: 0;
    .car-decrease{
        font-size: 24px;
    }
    .car-add{
        font-size: 24px;
    }
    .car-count{
        font-size: 10px;
        color: rgb(147, 153, 159);
        line-height: 24px;
        text-align: center;
        margin: 0 12px;
        vertical-align: top;
    }
}
</style>
