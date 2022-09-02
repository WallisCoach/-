<template>
  <el-table :data="userList" boder stripe>
  <!-- 基本信息-->
    <el-table-column type="index"></el-table-column>
    <el-table-column prop="username" label="姓名"></el-table-column>
    <el-table-column prop="email" label="邮箱"></el-table-column>
    <el-table-column prop="mobile" label="电话"></el-table-column>
    <el-table-column prop="role_name" label="角色"></el-table-column>

    <!-- 状态 -->
    <el-table-column label="状态">
        <template slot-scope="scope">
            <user-set-state :state="scope.row.mg_state" :userInfo="scope.row">
            </user-set-state>
        </template>
    </el-table-column>

    <!-- 操作 -->
    <el-table-column>
        <template slot-scope="scope">
            <!-- 修改按钮 -->
            <user-edit-dialog :user-id="scope.row.id" @user-list="updateUserList"></user-edit-dialog>

            <!-- 删除按钮 -->
            <user-remove-dialog :user-id="scope.row.id" @user-list="updateUserList"></user-remove-dialog>

            <!-- 分配角色按钮 -->
            <user-ser-role :role="scope.row" @user-list="updateUserList"></user-ser-role>
        </template>
    </el-table-column>
  </el-table>
</template>

<script>
import UserRemoveDialog from './UserRemoveDialog.vue'
import UseEditDialogVue from './UseEditDialog.vue';
import UserRemoveDialogVue from './UserRemoveDialog.vue';
import UserSetRoleVue from './UserSetRole.vue';
export default {

}
</script>

<style>

</style>
UserRemoveDialog