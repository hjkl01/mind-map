<template>
  <div class="workbencheContainer">
    <router-view></router-view>
  </div>
</template>

<script>
import axios from 'axios'
import { mapMutations } from 'vuex'
import { vipFileUrl, versionUrl } from '@/config/constant'
import pkg from '../../../package.json'
import { compareVersion } from 'simple-mind-map/src/utils'

export default {
  name: 'Workbenche',
  created() {
    document.title = '思绪思维导图'
    this.getVipInfo()
    this.getVersionInfo()
    this.$bus.$on('refreshVIP', this.getVipInfo)
    this.$bus.$on('check_update', this.getVersionInfo)
  },
  beforeDestroy() {
    this.$bus.$off('refreshVIP', this.getVipInfo)
    this.$bus.$off('check_update', this.getVersionInfo)
  },
  methods: {
    ...mapMutations(['setIsVIP']),

    // 获取会员信息
    async getVipInfo(showTip = false) {
      try {
        const clientUUID = await window.electronAPI.getClientUUID()
        const { data } = await axios.get(vipFileUrl + '?' + Date.now(), {
          responseType: 'json'
        })
        this.setIsVIP(data.includes(clientUUID))
        if (showTip) {
          this.$message.success('会员状态获取成功')
        }
      } catch (error) {
        console.log(error)
        this.$message.error('会员信息获取失败')
      }
    },

    // 获取版本信息
    async getVersionInfo(force = false, showNoTip = false) {
      try {
        const { data } = await axios.get(versionUrl + '?' + Date.now(), {
          responseType: 'json'
        })
        if (compareVersion(pkg.version, data.version) === '<') {
          const key = 'checkUpdateTip'
          const tipVersion = localStorage.getItem(key)
          if (!force && tipVersion === data.version) return
          localStorage.setItem(key, data.version)
          const h = this.$createElement
          this.$confirm('检查到有新版本，是否去下载？', '提示', {
            message: h('p', null, [
              h('p', { style: { color: '#F56C6C' } }, '检查到有新版本！'),
              h(
                'el-button',
                {
                  props: { size: 'mini' },
                  on: {
                    click: () => {
                      const { windows, mac, linux } = data.downloadAddress.baidu
                      const url = this.IS_WIN
                        ? windows
                        : this.IS_MAC
                        ? mac
                        : linux
                      window.electronAPI.openUrl(url)
                    }
                  }
                },
                '去百度网盘下载'
              ),
              h(
                'el-button',
                {
                  props: { size: 'mini' },
                  on: {
                    click: () => {
                      window.electronAPI.openUrl(data.downloadAddress.github)
                    }
                  }
                },
                '去Github下载'
              )
            ]),
            showConfirmButton: false,
            cancelButtonText: '关闭',
            type: 'warning'
          })
            .then(() => {})
            .catch(() => {})
        } else {
          if (showNoTip) {
            this.$message.info('当前已是最新版本~')
          }
        }
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>

<style lang="less" scoped>
.workbencheContainer {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}
</style>
