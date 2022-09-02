<template>
  <el-row
    :class="['border-bottom', index1 === 0 ? 'border-top' : '', 'vcenter']"
    v-for="(item1, index1) in scope.row.children"
    :key="item1.id"
  >
    <!-- 一级权限 -->
    <el-col :span="5">
      <el-tag closable @close="removeRightById">{{ item1.authName }}</el-tag>
      <i class="el-icon-caret-right"></i>
    </el-col>

    <!-- 二级和三级权限 -->
    <el-col :span="19">
      <el-row
        class="vcenter"
        :class="{ 'border-top': indexw !== 0 }"
        v-for="(item2, index2) in item.children"
        :key="item.id"
      >
        <el-col :span="6">
          <el-tag
            closable
            @close="removeRightByid(scope.row.item2.id)"
            type="success"
            >{{ item2.authName }}</el-tag
          >
          <i class="el-icon-caret-right"></i>
        </el-col>
        <el-col :span="18">
            <el-tag closable @close="removeRightById(scope.row.item3.id)" type="warning"
            v-for="(item3) in item2.children" :key="item3.id">{{item3.authName}}
            ></el-tag>
        </el-col>
      </el-row>
    </el-col>
  </el-row>
</template>

<script>
import {removeRightByIdRequest} from 'network/rights'
export default {
    name:'RoleTableRightRow',
    props:{
        scope:{
            type:Object,
        }
    },
    methods:{
        //根据ID删除对应的权限
        removeRightById(role,rightId){
            this.$confirm('此操作将删除改权限，是否继续','提示',{
                confirmButtonText:'确定',
                cancelButtonText:'取消',
                type:'warning'
            }).then(() =>{
                removeRightByIdRequest(role.id,rightId).then(res =>{
                    let result = res.data;

                    if(result.meta.status !== 200){
                        return this.alertMessage('删除失败','error');
                    }

                    this.alertMessage('删除成功','success');

                    //删除成功后会获得当前角色的最新权限，此时设置role的chilren属性讲进行更新，不会率先你页面
                    role.children = result.data;
                })
            }).catch(() =>{
                this.$message({
                    type:'info',
                    message:'已取消删除'
                })
            })
        }
    }
};
</script>

<style>  .el-tag {
    margin: 7px;
  }

  .border-top {
    border-top: 1px solid #eee;
  }

  .border-bottom {
    border-bottom: 1px solid #eee;
  }

  .vcenter {
    display: flex;
    align-items: center;
  }</style>
