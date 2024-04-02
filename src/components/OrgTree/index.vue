<template>
  <a-tree v-model:expandedKeys="expandedKeys" v-model:selectedKeys="selectedKeys" :load-data="onLoadData"
    :tree-data="treeData" :fieldNames="{ children: 'children', title: 'name', key: 'id' }" @select="treeSelect" />
</template>
<script lang="ts" name="OrgTree" setup>
import { ref, onMounted } from 'vue';
import type { TreeProps } from 'ant-design-vue';
import orgApi from '../../api/org'
import { Mitt } from '../../utils/mitt';

type OrgItem = {
  name: string,
  id: string,
  children?: OrgItem[]
}

const expandedKeys = ref<string[]>([]);
const selectedKeys = ref<string[]>([]);
const treeData = ref<OrgItem[]>([
  { name: 'Expand to load', id: '0' }
]);


const onLoadData: TreeProps['loadData'] = (treeNode) => {
  console.log(treeNode);
  return new Promise<void>(resolve => {
    orgApi.query(treeNode.dataRef?.id).then(org => {
      treeNode.dataRef.children = org;
      treeData.value = [...treeData.value];
      resolve()
    })

  });
};

const getTree = () => {
  orgApi.query().then((users) => {
    treeData.value = users
  })
}

const treeSelect = (treeNode: string[]) => {
  Mitt.emit('OrgChange', treeNode[0])
}

onMounted(() => {
  getTree()
})
</script>
<style lang="scss"></style>