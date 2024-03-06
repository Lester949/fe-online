<script lang="ts" setup>
import orgApi from "../api/org";
import type Node from "element-plus/es/components/tree/src/model/node";
interface Tree {
  id: string;
  name: string;
}

const defaultProps = {
  label: "name",
  children: "children",
};

const emit = defineEmits<{
  handleNodeClick: [id: string];
}>();

// 根据父节点id加载
const loadData = async (node: Node, resolve: (data: Tree[]) => void) => {
  const parentId = node.data.id;
  const res: Tree[] = await orgApi.query(parentId);
  return resolve(res);
};
// 节点事件
const handleNodeClick = (data: Tree) => {
  emit("handleNodeClick", data.id);
};
</script>
<template>
  <div class="treeBox">
    <el-tree
        lazy
        :highlight-current="true"
        :props="defaultProps"
        :load="loadData"
        @node-click="handleNodeClick"
    />
  </div>
</template>
<style scoped>
.treeBox {
  border-right: 1px solid #eee;
  width: 200px;
  height: 100vh;
}
</style>
