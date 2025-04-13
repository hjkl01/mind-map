<template>
  <div class="workbencheHelpContainer">
    <div class="workbencheHelpHeader">
      <MacControl></MacControl>
      <Name v-if="IS_WIN"></Name>
      <div class="rightBar">
        <WinControl></WinControl>
        <Name :isRight="true" v-if="IS_MAC"></Name>
      </div>
    </div>
    <div class="workbencheHelpContent customScrollbar">
      <div class="title">使用教程</div>
      <div class="helpList">
        <div class="helpItem" v-for="item in helpList">
          <div class="helpTitle">
            <span class="icon el-icon-document"></span>
            <span class="text">{{ item.title }}</span>
          </div>
          <div class="helpContentList">
            <div
              class="helpContentItem"
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
                >{{ subItem.title }}</el-link
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import WinControl from '../components/WinControl.vue'
import MacControl from '../components/MacControl.vue'
import { mapState } from 'vuex'
import axios from 'axios'
import { helpFileUrl } from '@/config/constant'
import Name from '../components/Name.vue'

export default {
  components: {
    WinControl,
    MacControl,
    Name
  },
  data() {
    return {
      helpList: []
    }
  },
  created() {
    this.getHelpInfo()
    document.title = '思维导图 - 使用帮助'
  },
  methods: {
    async getHelpInfo() {
      try {
        const { data } = await axios.get(helpFileUrl + '?' + Date.now(), {
          responseType: 'json'
        })
        this.helpList = data
      } catch (error) {
        console.log(error)
      }
    },

    openLink(url) {
      window.electronAPI.openUrl(url)
    }
  }
}
</script>

<style lang="less" scoped>
.workbencheHelpContainer {
  background-color: #f6f8f9;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;

  .workbencheHelpHeader {
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

  .workbencheHelpContent {
    width: 100%;
    height: 100%;
    padding: 20px;
    overflow-y: auto;

    .title {
      height: 100px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 25px;
      font-weight: bold;
    }

    .helpList {
      max-width: 800px;
      margin: 0 auto;

      .helpItem {
        margin-bottom: 20px;

        .helpTitle {
          color: #1a1a1a;
          font-size: 16px;
          font-weight: 600;
          display: flex;
          align-items: center;
          margin-bottom: 12px;

          .icon {
            margin-right: 4px;
          }
        }

        .helpContentList {
          .helpContentItem {
            p {
              margin-bottom: 8px;
              color: #333;
            }
          }
        }
      }
    }
  }
}
</style>
