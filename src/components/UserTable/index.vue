<template>
    <a-form>
        <a-form-item label="关键字">
            <a-input v-model:value="keyword" @change="handleChange" placeholder="请输入关键字"></a-input>
        </a-form-item>
    </a-form>
    <a-table bordered :dataSource="userList" :columns="columns" :pagination="false" :scroll="{ y: 400 }" />
</template>
<script lang="ts" name="OrgTree" setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import userApi from '../../api/user';
import { Mitt } from '../../utils/mitt';
import { debounce } from 'lodash'

type User = {
    name: string,
    id: string
}

const keyword = ref('')

const columns = [
    {
        title: '序号',
        dataIndex: 'index',
        key: 'index',
        width: 70,
        customRender: ({ index }: { index: number }) => index + 1
    },
    {
        title: 'id',
        dataIndex: 'id',
        key: 'id',
    },
    {
        title: '姓名',
        dataIndex: 'name',
        key: 'name',
    }]
const userList = ref<User[]>([])

const getList = (orgId?:string) => {
    userApi.query({orgId}).then((users) => {
        userList.value = users
    })
}

const handleChange = debounce((value:string) => {
    getList(value)
},1000)


onMounted(() => {
    getList()
    Mitt.on('OrgChange', (orgId) => {
        getList(orgId as string)
    })
})
onBeforeUnmount(()=>{
    Mitt.off('OrgChange')
})
</script>
<style lang="scss"></style>