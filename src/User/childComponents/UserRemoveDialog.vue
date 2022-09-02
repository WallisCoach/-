<template>
  <!-- 删除用户的按钮 -->
  <el-button type="danger" icon="el-icon-delete" size="mini" @click="removeUserById(userId)"></el-button>
</template>

<script>
import {removeUserByIdRequst} from ''
export default {
    name:'UserRemoveDialog',
    props:{
        userId:{
            type:Number,
            default:0
        }
    },
    data() {
        return {
            
        } 
    },
    methods:{
        removeUserById(id){
            this.$confirm('此操作将删除改用户，是否继续？','提示',{
                confirmButtonText:'确定',
                cancelButtonText:'取消',
                type:'warning'
            }).then(()=>{
                removeUserByIdRequest(id).then(res =>{
                    let result = res.data;
                    if (result.meta.status !== 200){
                        return this.alertMessage('删除失败','error');
                    }
                    this.alertMessage('删除成功','success');

                    this.$emit('user-list');
                });
            }).catch(() =>{
                this.alertMessage('已取消删除','info')
            })
        }
    }
}
</script>

<style>

</style>