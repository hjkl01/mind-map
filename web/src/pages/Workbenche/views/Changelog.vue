<template>
  <div class="workbencheChangelogContainer">
    <div class="workbencheChangelogHeader">
      <MacControl></MacControl>
      <Name v-if="IS_WIN"></Name>
      <div class="rightBar">
        <WinControl></WinControl>
        <Name :isRight="true" v-if="IS_MAC"></Name>
      </div>
    </div>
    <div class="workbencheChangelogContent customScrollbar">
      <div class="title">更新记录</div>
      <div class="changelogList">
        <div class="changeItem" v-for="item in changelogList">
          <div class="changeTitle">
            <span class="icon el-icon-document"></span>
            <span class="text">{{ item.title }}</span>
          </div>
          <div class="changelogContentList">
            <div class="listTitle">新增功能：</div>
            <div
              class="changelogContentItem"
              v-for="(subItem, index) in item.newList"
              :key="index"
            >
              <p>{{ index + 1 }}.{{ subItem }}；</p>
            </div>
          </div>
          <div class="changelogContentList">
            <div class="listTitle">修复bug：</div>
            <div
              class="changelogContentItem"
              v-for="(subItem, index) in item.fixList"
              :key="index"
            >
              <p>{{ index + 1 }}.{{ subItem }}；</p>
            </div>
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
import { changelogUrl } from '@/config/constant'
import Name from '../components/Name.vue'

export default {
  components: {
    WinControl,
    MacControl,
    Name
  },
  data() {
    return {
      changelogList: []
    }
  },
  created() {
    this.getChangelogInfo()
    document.title = '思维导图 - 更新记录'
  },
  methods: {
    async getChangelogInfo() {
      try {
        const { data } = await axios.get(changelogUrl + '?' + Date.now(), {
          responseType: 'json'
        })
        this.changelogList = data
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
.workbencheChangelogContainer {
  background-color: #f6f8f9;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;

  .workbencheChangelogHeader {
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

  .workbencheChangelogContent {
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

    .changelogList {
      max-width: 800px;
      margin: 0 auto;

      .changeItem {
        margin-bottom: 20px;

        .changeTitle {
          color: #1a1a1a;
          font-size: 18px;
          font-weight: 600;
          display: flex;
          align-items: center;
          margin-bottom: 12px;

          .icon {
            margin-right: 4px;
          }
        }

        .changelogContentList {
          .listTitle {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 10px;
          }

          .changelogContentItem {
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
