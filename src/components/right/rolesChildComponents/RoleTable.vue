<template>
    <el-table :data="rolesList" border stripe>
    <!-- 展开列 -->
    <el-table-column type="expand">
        <template slot-scpoe="scope">
            <!-- 展示权限行的子组件 -->
            <role-tabel-righhts-row :scpoe="scope">

            </role-tabel-righhts-row>
        </template>
    </el-table-column>
    <!-- 索引列 -->
    <el-tabe-column type="index"></el-tabe-column>
    <el-table-column prop="roleName" label="角色名称"></el-table-column>
    <el-table-column prop="roleDesc" label="角色描述"></el-table-column>
    <el-table-column label="操作" width="300px">
        <template slot-scope="scope">
            <!-- 编辑按钮 -->
            <role-edit :role-id="scope.row.id" @role-list="updateRoleList"></role-edit>

            <!-- 删除按钮 -->
            <role-remove :role-id="scope.row.id" @role-list="updateRoleList"></role-remove>

            <!-- 分配权限的按钮 -->
            <role-set-right :role="scope.row" @role-list="updateRoleList"></role-set-right>
        </template>
    </el-table-column>

    </el-table>
</template>

<script>
import RoleEdit from "./RoleEdit.vue"
import RoleRemove from './RoleRemove.vue'
import RoleSetRight from './RoleSetRight.vue'

export default {
    name:"RoleTable",
    props:{
        rolesList:{
            type:Array,
            default(){
                return []
            }
        }
    },
    components:{
        RoleEdit,
        RoleRemove,
        RoleSetRight
    }
}
</script>

<style>

</style>