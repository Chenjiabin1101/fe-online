<template>
    <div id="app">
        <a-row>
            <a-col :span="6">
                <OrgTree></OrgTree>
            </a-col>
            <a-col :span="18">
                <UserTable></UserTable>
            </a-col>
        </a-row>

    </div>
</template>
<script lang="ts" name="app" setup>
import OrgTree from './components/OrgTree/index.vue'
import UserTable from './components/UserTable/index.vue'
</script>
<style lang="scss"></style>