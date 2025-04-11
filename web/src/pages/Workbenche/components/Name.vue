<template>
  <div class="softwareName">
    <span class="text">思绪思维导图</span>
    <span class="subText">v{{ version }}</span>
  </div>
</template>

<script>
import pkg from '../../../../package.json'

export default {
  data() {
    return {
      version: pkg.version
    }
  }
}
</script>

<style lang="less" scoped>
.softwareName {
  display: flex;
  align-items: baseline;
  padding-left: 12px;

  .text {
    font-weight: bold;
    font-size: 16px;
  }

  .subText {
    font-size: 14px;
    margin-left: 4px;
  }
}
</style>
