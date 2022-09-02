<template>
  <div>
    <el-row>
      <el-col>
        <el-button type="primary">添加分类</el-button>
      </el-col>
    </el-row>

    <!-- 添加分类的对话框 -->
    <el-dialog
      title="提示"
      :visible.sync="addDialogVisible"
      width="50%"
      @close="addCategoryDialogVisible"
    >
      <el-form
        :model="addCategoryForm"
        :rules="categoryFormRules"
        ref="addCategoryFormRef"
        labe-width="100px"
      >
        <el-form-item label="分类名称" prop="cat_name">
            <el-input v-model="addCategoryForm.cat_name"></el-input> 
             </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
export default {};
</script>

<style></style>
