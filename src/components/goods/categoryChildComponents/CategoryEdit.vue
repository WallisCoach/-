<template>
  <div class="editDialog">
    <el-button type="primary" size="mini" icon="el-icon-edit" @click="showEditDialog(id)">
        编辑
    </el-button>

    <el-dialog title="编辑" :visible.sync="editDialogVisible" width="50%" @close="editDialogClose">
        <el-form :model="editCategoryForm" :rules="categoryFormRules" ref="editCateFormRef" label-width="80px"></el-form>
    </el-dialog>
  </div>
</template>

<script>
import {getCategoryByIdRequest,editCategoryByIdRequest}from ''
export default {
    name:'CategoryEdit',
    props:{
        id:{
            type:Number,
            default:0,
        }
    },
    data(){
        return{
            editDialogVisible:false,
            categoryFormRules:{
                cat_name:[
                    {required:true, message:'请输入分类名称',trigger:'blur'}
                ]
            },
            editCategoryForm:{
                cat_name:''
            }
        }
    },
    methods:{
        //编辑
        showEditDialog(id){
            getCategoryByIdRequest(id).then(res =>{
                let result = res.data;
                if (result.meta.status !== 200){
                    return this.alertMessage('获取消息失败','error');
                }

                this.editCategoryForm = result.data;
                this.editDialogVisible = true;
            })
        },

        //监听编辑的弹框的关闭事件
        editCategory(){
            this.$refs.editCategoryFormRef.resetFields();
            },

        //点击按钮确定编辑
        editCategory(){
            this.$ref.editCategoryFormRef.validate(valid =>{
                if(!valid) return;
                editCategoryByIdRequest(this.editCategoryForm).then(valid =>{
                    let result = res.data;
                    if(result.meta.status !== 200){
                        return this.alertMssage('修改失败','error');
                    }
                    this.alertMessage('修改成功','success')

                    //隐藏弹窗
                    this.editDialogVisible = false;

                    //重新获取角色列表
                    this.$emit('category-list');
                })
            })
        }
    }
}
</script>

<style scoped>
    .editDialog {
      display: inline-block;
      margin-right: 5px;
    }
</style>