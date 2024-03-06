<script lang="ts" setup>
import { ref } from "vue";
import OrgTree from "./components/OrgTree.vue";
import UserTable from "./components/UserTable.vue";
import type { TabsPaneContext } from 'element-plus'

const activeName = ref('first')

//选项卡事件
const handleClick = (tab: TabsPaneContext, event: Event) => {
  console.log(tab, event)
}
//获取table组件实例
const UserTableRef = ref<typeof UserTable>();
// 树节点事件
const handleNodeClick = (id: string) => {
  //直接调用user组件的方法
  UserTableRef.value!.getUserList("orgId",id);
};
</script>

<template>
  <div class="app">
    <el-tabs v-model="activeName" class="demo-tabs" @tab-click="handleClick">
      <el-tab-pane label="成员管理" name="first">
        <div class="content">
          <OrgTree @handleNodeClick="handleNodeClick" />
          <UserTable ref="UserTableRef"/>
        </div>
      </el-tab-pane>
      <el-tab-pane label="团队管理" name="second">团队管理</el-tab-pane>
      <el-tab-pane label="职位维护" name="third">职位维护</el-tab-pane>
    </el-tabs>




  </div>
</template>

<style scoped>
.app {
  display: flex;
}
.content{
  display: flex;
  justify-content: space-between;
}
.demo-tabs  {
 width: 100%;
}
</style>
