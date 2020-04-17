<template>
    <span class="icon" :class="iconClassMap[iconType]"></span>
</template>
<script type="text/ecmascript-6">
export default {
  data() {
    return {
    };
  },
  props: ['iconType'],
  created() {
   this.iconClassMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  }
};
</script>
<style lang="less">
@import '../../common/less/mixin.less';
     .icon {
        display: inline-block;
        width: 12px;
        height: 12px;
        // background: blue;
        background-size: 12px 12px;
        background-repeat: no-repeat;
        vertical-align: bottom;
        &.decrease {
          .bg-image(decrease_3);
        }
        &.discount {
          .bg-image(discount_3);
          // background: url('./img/decrease_1@2x.png');
        }
        &.guarantee {
          // background: url('./img/guarantee_1@2x.png');
           .bg-image(guarantee_3);
        }
        &.invoice {
          //  background: url('./img/invoice_1@2x.png');
          .bg-image(invoice_3);
        }
        &.special {
          //  background: url('./img/special_1@2x.png');
          .bg-image(special_3);
        }
      }
</style>
