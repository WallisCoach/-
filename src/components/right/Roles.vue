<template>
  <div>
    <!-- 面包屑导航 -->
    <breadcrumb-nav >
    <template v-slot:firstMenu>权限管理</template>
    <template v-slot:sencondMenu>角色列表</template>
    </breadcrumb-nav>

    <!-- 卡片视图 -->
    <el-card>
        <!-- 添加橘色 -->
        <role-add @role-list="updateRoleList"></role-add>
        
        <!-- 角色表格 -->
        <role-table :role-list="roleList" @role-list="updateRoleList"></role-table> 
    </el-card>
  </div>
</template>

<script >
export default {
name:'Roles',
components:{
    BreadcrumbNav,
    RoleAdd,
    RoleEdit,
    RoleRemove,
    RoleSetRight,
    RoleTable
},
data(){
    return{
        roleList:[],
    }
},
created(){
    this.getRolesList();
},
methods:{
    getRoleList(){
        getRolesListRequest().then(res =>{
            let result = res.data;
            if (result.meta.status !== 200){
                return this.alertMseeage('获取角色列表失败','error');
            }
            this.rolesList = result.data
        })
    },

    //监听子组件中发出的事件，重新获取角色列表
    updateRoleList(){
        this.getRolesList();
    }
}
}
</script>

<style>

</style>