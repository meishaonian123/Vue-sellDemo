<template>
    <div class="star" :class="starType">
        <span class="star-item" v-for="(itemClass,index) in itemClasses" :class="itemClass" :key="index"></span>
    </div>
</template>
<script type="text/ecmascript-6">
const LENGTH = 5;
const CLS_ON = 'on';
const CLS_OFF = 'off';
const CLS_HALF = 'half';
export default {
  data () {
    return {
    };
  },
  props: ['size', 'score'],
  computed: {
      starType() {
          return 'star-' + this.size;
      },
      itemClasses() {
          let result = [];
          let score = Math.floor(this.score * 2) / 2; // 计算所有星星的数量
          let hasDecimal = score % 1 !== 0; // 非整数星星判断
          let integer = Math.floor(score); // 整数星星判断
          for (let i = 0; i < integer; i++) {
             result.push(CLS_ON);
          }
          if (hasDecimal) {
              result.push(CLS_HALF);
          }
          while (result.length < LENGTH) {
              result.push(CLS_OFF);
          }
          return result;
      }
  }
};
</script>
<style lang="less">
@import '../../common/less/mixin.less';
.star{
    &.star-48{
        .star-item{
            width: 20px;
            height: 20px;
            &.on{
                .bg-image(star48_on);
            }
            &.half{
                .bg-image(star48_half);
            }
            &.off{
                .bg-image(star48_off);
            }
        }
    }
     &.star-24{
         text-align: center;
        .star-item{
            width: 20px;
            height: 20px;
            display: inline-block;
            background-size: 20px 20px;
            background-repeat: no-repeat;
            margin-right: 22px;
            margin-top: 16px;
            &:last-child{
                 margin-right: 0;
            }
            &.on{
                .bg-image(star24_on);
            }
            &.half{
                .bg-image(star24_half);
            }
            &.off{
                .bg-image(star24_off);
            }
        }
    }
     &.star-36{
         margin: 0 12px;
        .star-item{
            width: 15px;
            height: 15px;
            display: inline-block;
            background-size: 15px 15px;
            background-repeat: no-repeat;
            margin-right: 5px;
            // margin-top: 11px;
            vertical-align: top;
            &:last-child{
                 margin-right: 0;
            }
            &.on{
                .bg-image(star36_on);
            }
            &.half{
                .bg-image(star36_half);
            }
            &.off{
                .bg-image(star36_off);
            }
        }
    }
}
</style>
