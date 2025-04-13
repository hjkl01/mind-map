<template>
  <div class="workbencheVipContainer">
    <div class="workbencheVipHeader">
      <MacControl></MacControl>
      <Name v-if="IS_WIN"></Name>
      <div class="rightBar">
        <WinControl></WinControl>
        <Name :isRight="true" v-if="IS_MAC"></Name>
      </div>
    </div>
    <div class="workbencheVipContent customScrollbar">
      <div class="content">
        <div class="vipStatus" :class="{ isVIP: isVIP }">
          <span class="iconfont iconhuiyuan-"></span
          >{{ isVIP ? '恭喜，' : '很遗憾，' }}您{{
            isVIP ? '已' : '还不'
          }}是思绪会员~
        </div>
        <div class="groupTitle">
          <span class="line"></span>
          <span class="text">开通会员</span>
          <span class="line"></span>
          <span
            class="expandIcon el-icon-arrow-down"
            :class="{ top: !showBuyBox }"
            @click="showBuyBox = !showBuyBox"
          ></span>
        </div>
        <div class="buyBox" v-show="showBuyBox">
          <p>
            思绪会员为<span class="emphasize">终身制</span
            >，后续所有新出的功能均无需再次付费。目前价格为：<span
              class="emphasize"
              >￥100</span
            >（后续可能会涨价哦，有需要的朋友建议早开早享受~）。最多允许三台电脑使用。
          </p>
          <p>
            <strong>购买方式：</strong>扫码进行支付，请备注<span
              class="emphasize"
              >会员购买</span
            >，支付完成后，可以手动将下面的<span
              class="btn"
              @click="doCopy(uuid)"
              >机器码</span
            >发送到指定邮箱<span class="btn" @click="doCopy(mailAddress)">{{
              mailAddress
            }}</span
            >。
            <!-- 也可以直接点击：<span class="btn" @click="sendMail">一键发送</span
          >。 -->
          </p>
          <div class="desc">
            <p>
              最多可发送三个机器码，您可以打开其他需要使用的电脑安装思绪思维导图，然后打开会员弹窗复制机器码一并进行发送。当然，也可以日后再单独发送。
            </p>
            <p>
              如果您觉得发邮件比较麻烦，也可以直接微信添加：<span
                class="btn"
                @click="doCopy('wanglinguanfang')"
                >wanglinguanfang</span
              >。然后直接发送机器码即可。
            </p>
          </div>
          <p>
            <strong>支付方式：</strong>
            <span class="btn" @click="openPayDialog('alipay')"
              >点此支付宝支付</span
            >
            或
            <span class="btn" @click="openPayDialog('wechat')"
              >点此微信支付</span
            >
          </p>
          <p class="row">
            <span class="text">机器码：</span>
            <el-input v-model="uuid" size="small" readonly>
              <el-button slot="append" @click="doCopy(uuid)">复制</el-button>
            </el-input>
          </p>
          <p class="row">
            <span class="text">邮箱地址：</span>
            <el-input v-model="mailAddress" size="small" readonly>
              <el-button slot="append" @click="doCopy(mailAddress)"
                >复制</el-button
              >
            </el-input>
          </p>
          <div class="desc">
            <p>
              温馨提醒：因为是人工录入，所以存在一定延迟，录入完成后会发送提醒邮件，当您收到邮件后可以尝试重新启动客户端，或者点击：<span
                class="btn"
                @click="refreshVIP"
                >刷新会员状态</span
              >。
            </p>
            <p>
              如有疑问，可添加微信咨询：<span
                class="btn"
                @click="doCopy('wanglinguanfang')"
                >wanglinguanfang</span
              >。
            </p>
          </div>
        </div>
        <div class="groupTitle">
          <span class="line"></span>
          <span class="text">会员功能</span>
          <span class="line"></span>
          <span
            class="expandIcon el-icon-arrow-down"
            :class="{ top: !showVipBox }"
            @click="showVipBox = !showVipBox"
          ></span>
        </div>
        <div class="vipBox" v-show="showVipBox">
          <div class="functionItem" v-for="item in functionList">
            <div class="functionTitle" v-if="item.title">
              <img v-if="item.titleIcon" :src="item.titleIcon" alt="">
              <span class="icon iconfont iconhuiyuan-"></span>
              <span class="text">{{ item.title }}</span>
            </div>
            <div class="functionContentList">
              <div
                class="functionContentItem"
                v-for="(subItem, index) in item.contentList"
                :key="index"
              >
                <p v-if="typeof subItem === 'string'">{{ subItem }}</p>
                <div
                  class="imgList"
                  v-else-if="subItem.type === 'img' && subItem.list.length > 0"
                >
                  <el-image
                    v-for="(img, imgIndex) in subItem.list"
                    :key="imgIndex"
                    :src="img"
                    :preview-src-list="subItem.list"
                    :initial-index="imgIndex"
                    style="width: 100px; height: 100px; margin-right: 10px;"
                    fit="cover"
                  >
                  </el-image>
                </div>
                <el-link
                  type="primary"
                  v-else-if="subItem.type === 'link'"
                  @click="openLink(subItem.url)"
                  style="margin-bottom: 12px;"
                  >{{ subItem.title }}</el-link
                >
              </div>
            </div>
          </div>
          <div>更多功能，持续更新。。。</div>
        </div>
      </div>
    </div>
    <el-dialog
      class="qrCodeDialog"
      :title="showPayType === 'alipay' ? '支付宝扫码' : '微信扫码'"
      :visible.sync="qrCodeDialogVisible"
      width="400px"
      @close="qrCodeDialogVisible = false"
    >
      <div>
        <img
          src="../../../assets/img/alipay.jpg"
          alt=""
          v-if="showPayType === 'alipay'"
        />
        <img
          src="../../../assets/img/wechat.jpg"
          alt=""
          v-if="showPayType === 'wechat'"
        />
      </div>
    </el-dialog>
  </div>
</template>

<script>
import WinControl from '../components/WinControl.vue'
import MacControl from '../components/MacControl.vue'
import { mapState } from 'vuex'
import axios from 'axios'
import { vipFunctionUrl } from '@/config/constant'
import Name from '../components/Name.vue'
import { copy } from '@/utils'

export default {
  components: {
    WinControl,
    MacControl,
    Name
  },
  data() {
    return {
      uuid: '',
      qrCodeDialogVisible: false,
      mailAddress: '1013335014@qq.com',
      showPayType: '',
      showBuyBox: false,
      functionList: [],
      showVipBox: true
    }
  },
  watch: {
    isVIP(val) {
      if (val) {
        this.showBuyBox = false
      }
    }
  },
  created() {
    document.title = '思维导图 - 会员'
    this.showBuyBox = !this.isVIP
    this.getUUID()
    this.getVipFunctionInfo()
  },
  methods: {
    async getUUID() {
      try {
        if (this.uuid) return
        const clientUUID = await window.electronAPI.getClientUUID()
        this.uuid = clientUUID
      } catch (error) {
        console.log(error)
      }
    },

    async getVipFunctionInfo() {
      try {
        const { data } = await axios.get(vipFunctionUrl + '?' + Date.now(), {
          responseType: 'json'
        })
        this.functionList = data
      } catch (error) {
        console.log(error)
      }
    },

    openPayDialog(type) {
      this.showPayType = type
      this.qrCodeDialogVisible = true
    },

    doCopy(text) {
      copy(text)
      this.$message.success('复制成功')
    },

    refreshVIP() {
      this.$bus.$emit('refreshVIP', true)
    },

    async sendMail() {
      const mailtoUrl = `mailto:${
        this.mailAddress
      }?subject=${encodeURIComponent('会员购买')}&body=${encodeURIComponent(
        this.uuid
      )}`
      try {
        await window.electronAPI.openExternal(mailtoUrl)
      } catch (error) {
        console.log(error)
        this.$message.error('一键发送失败，请手动发送')
      }
    }
  }
}
</script>

<style lang="less" scoped>
.qrCodeDialog {
  /deep/ .el-dialog__body {
    padding: 0;
  }

  div {
    width: 100%;
    padding: 20px;
  }

  img {
    width: 100%;
  }
}

.workbencheVipContainer {
  background-color: #f6f8f9;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;

  .workbencheVipHeader {
    position: relative;
    width: 100%;
    height: 40px;
    background-color: #ebeef1;
    -webkit-app-region: drag;
    display: flex;
    align-items: center;
    flex-shrink: 0;

    .rightBar {
      -webkit-app-region: no-drag;
      position: absolute;
      right: 0px;
      top: 0;
      height: 100%;
      display: flex;
      align-items: center;
    }
  }

  .workbencheVipContent {
    width: 100%;
    height: 100%;
    padding: 20px;
    overflow-y: auto;

    .content {
      max-width: 800px;
      margin: 0 auto;
      padding: 20px 0;

      .vipStatus {
        display: flex;
        align-items: center;
        justify-content: center;
        margin-bottom: 30px;
        width: 100%;
        font-size: 25px;
        font-weight: bold;
        color: #f56c6c;
        display: flex;
        align-items: center;
        flex: 1;

        &.isVIP {
          color: #e6a23c;
        }

        span {
          margin-right: 10px;
          font-size: 25px;
        }
      }

      .groupTitle {
        display: flex;
        align-items: center;
        margin-bottom: 20px;
        overflow: hidden;
        justify-content: center;
        margin-top: 50px;

        .line {
          width: 200px;
          height: 1px;
          background-color: #e3e4e6;
        }

        .text {
          font-size: 16px;
          font-weight: bold;
          flex-shrink: 0;
          margin: 0 20px;
        }

        .expandIcon {
          margin-left: 4px;
          cursor: pointer;
          font-size: 16px;
          transition: all 0.3s;

          &.top {
            transform: rotateZ(180deg);
          }
        }
      }

      .buyBox {
        background-color: #fff;
        border-radius: 10px;
        padding: 12px;

        .desc {
          padding: 12px;
          background-color: #f5f5f5;
          margin: 12px 0;
          font-size: 14px;
        }

        .row {
          display: flex;
          align-items: center;
        }

        p {
          margin-bottom: 10px;

          &:last-of-type {
            margin-bottom: 0;
          }

          .text {
            flex-shrink: 0;
          }

          a {
            color: #409eff;
            font-weight: bold;
          }

          .btn {
            color: #409eff;
            cursor: pointer;
            font-weight: bold;
          }

          .emphasize {
            font-weight: bold;
            color: #409eff;
          }
        }
      }

      .vipBox {
        background-color: #fff;
        border-radius: 10px;
        padding: 12px;

        .functionItem {
          margin-bottom: 20px;

          .functionTitle {
            color: #1a1a1a;
            font-size: 16px;
            font-weight: 600;
            display: flex;
            align-items: center;
            margin-bottom: 12px;
            color: #e6a23c;

            img {
              width: 30px;
              margin-right: 4px;
            }

            .icon {
              margin-right: 4px;
              color: #e6a23c;
            }
          }

          .functionContentList {
            .functionContentItem {
              p {
                margin-bottom: 8px;
                color: #333;
              }

              .imgList {
              }
            }
          }
        }
      }
    }
  }
}
</style>
