<template>
  <span class="vipMark"> </span>
</template>

<script>
export default {}
</script>

<style lang="less" scoped>
.vipMark {
  display: inline-block;
  width: 24px;
  height: 13px;
  overflow: hidden;
  background-image: url('../../../assets/img/vip.png');
  background-size: cover;

  i {
    font-size: 26px !important;
    color: rgb(175, 116, 63) !important;
    display: inline-block;
    margin-left: -2px;
    margin-top: -1px;
  }
}
</style>
