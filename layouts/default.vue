<template>
  <div class="content">
    <div class="table-content">
      <el-tabs v-model="activeTab" class="setting-content">
        <el-tab-pane v-for="(item, index) in panes" :key="index" :label="item.label" :name="item.name">
          <pane-content v-if="activeTab === item.name" :activeTab="activeTab" />
        </el-tab-pane>
      </el-tabs>
    </div>
  </div>
</template>

<script>
import paneContent from './paneContent'
export default {
  components: {
    paneContent
  },
  data() {
    return {
      activeTab: 'build',
      panes: [
        { label: 'Build', name: 'build' },
        { label: 'Section', name: 'section' }
      ]
    }
  }
}
</script>

<style scoped>
.content {
  height: 100%;
  min-width: 1000px;
  padding: 0 20px 10px 20px;
  overflow-y: auto;
  overflow-x: hidden;
}
.table-content {
  width: 100%;
  margin-top: 10px;
  /* padding: 10px; */
  box-shadow: 0px 2px 3px #00000029;
  border: 1px solid #dcdfe6;
  border-radius: 20px;
  overflow: hidden;
}
.table-content /deep/ .el-tabs__item {
  font-weight: 700;
  color: #1592E6;
}
.table-content /deep/ .el-tabs__item.is-active {
  color: #1592E6;
}
.table-content /deep/ .el-tabs__active-bar {
  background-color: #1592E6;
}
.table-content /deep/ .el-tabs__header {
  padding: 0 10px;
  margin: 0;
  background: #f0efef;
}
</style>
