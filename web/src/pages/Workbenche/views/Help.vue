<template>
  <div class="workbencheHelpContainer" :class="{ isDark: isDark }">
    <div class="workbencheHelpHeader">
      <MacControl></MacControl>
      <div class="rightBar">
        <WinControl></WinControl>
      </div>
    </div>
    <div class="workbencheHelpContent customScrollbar">
      <div v-for="item in helpList">
        <div class="title">{{ item.title }}</div>
        <div class="pList">
          <p v-for="text in item.textList">{{ text }}</p>
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

export default {
  components: {
    WinControl,
    MacControl
  },
  data() {
    return {
      helpList: []
    }
  },
  computed: {
    ...mapState({
      isDark: state => state.localConfig.isDark
    })
  },
  created() {
    this.getHelpInfo()
  },
  methods: {
    async getHelpInfo() {
      try {
        const { data } = await axios.get(helpFileUrl + '?' + Date.now(), {
          responseType: 'json'
        })
        console.log(data)
        this.helpList = data
      } catch (error) {
        console.log(error)
      }
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

  &.isDark {
    background: rgb(39, 42, 46);

    .workbencheHelpHeader {
      background-color: #262a2e;

      .rightBar {
      }
    }
  }

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
  }
}
</style>
