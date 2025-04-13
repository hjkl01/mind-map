<template>
  <div class="softwareName" :class="{ isRight: isRight, isDark: isDark }">
    <span class="text">思绪思维导图</span>
    <span class="subText">v{{ version }}</span>
  </div>
</template>

<script>
import pkg from '../../../../package.json'
import { mapState } from 'vuex'

export default {
  props: {
    isRight: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      version: pkg.version
    }
  },
  computed: {
    ...mapState({
      isDark: state => state.localConfig.isDark
    })
  },
}
</script>

<style lang="less" scoped>
.softwareName {
  display: flex;
  align-items: baseline;
  padding-left: 12px;

  &.isRight {
    padding-right: 12px;
  }

  &.isDark {
    color: #fff;
  }

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
