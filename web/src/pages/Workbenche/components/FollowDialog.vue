<template>
  <el-dialog
    class="sponsorDialog"
    title="关注我们"
    :visible.sync="dialogVisible"
    width="480px"
    @close="onClose"
  >
    <div class="sponsorBox">
      <p>可以通过如下渠道关注我们，获取最新软件动态。</p>
      <div class="imgList">
        <div>
          <img src="../../../assets/img/gzh.jpg" alt="" />
          <div class="name">微信公众号</div>
        </div>
        <div>
          <img src="../../../assets/img/xhs.jpg" alt="" />
          <div class="name">小红书</div>
        </div>
      </div>
    </div>
  </el-dialog>
</template>

<script>
export default {
  model: {
    prop: 'value',
    event: 'change'
  },
  props: {
    value: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      dialogVisible: false
    }
  },
  watch: {
    value(val, oldVal) {
      this.dialogVisible = val
    }
  },
  methods: {
    onClose() {
      this.$emit('change', false)
    },

    open(type) {
      let url = ''
      switch (type) {
        case 'github':
          url =
            'https://github.com/wanglin2/mind-map#%E8%AF%B7%E4%BD%9C%E8%80%85%E5%96%9D%E6%9D%AF%E5%92%96%E5%95%A1'
          break
        default:
          break
      }
      window.electronAPI.openUrl(url)
    }
  }
}
</script>

<style lang="less" scoped>
.sponsorDialog {
  /deep/ .el-dialog__body {
    padding: 0;
  }
}

.sponsorBox {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 30px;

  img {
    width: 200px;
  }

  h2 {
    margin-bottom: 20px;
  }

  p {
    margin-bottom: 10px;

    span {
      cursor: pointer;
      color: #409eff;
      margin-right: 10px;

      &:last-of-type {
        margin-right: 0;
      }
    }
  }

  .imgList {
    display: flex;

    .name {
      text-align: center;
    }
  }
}
</style>
