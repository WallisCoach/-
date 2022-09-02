<template>
  <div>
    <!-- 卡片试图区域 -->
    <el-card>
        <category-add @category-list="updateCategoryList"></category-add>
    
    <!-- 表格 -->
    <category-table :category-list="categorylist" @category-list="updateCategoryList"></category-table>
    
    <!-- 分页区域 -->
    <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="queryInfo.pagenum"
                :page-sizes="[2,5,7,10]"
                :page-size="queryInfo.pagesize"
                layout="total.sizes,prev,pager,next,jumper"
                :total="total">
    </el-pagination>
    </el-card>
  </div>
</template>

<script>
import CategoryAdd from './CategoryAdd.vue';
import CategoryTable from '';
import {getCategoriesListRequest} from ''
export default {
    name:'CategoryCard',
    components:{
        CategoryAdd,
        CategoryTable
    },
    data(){
        return{
            queryInfo:{
                type:3,
                pagenum:1,
                pagesize:2
            },
            categoryList:[],
            total:0,
        }
    },
    created(){
        this.getCategoriesList()
    },
    methods:{
        //获取商品分类
        getCategoriesList(){
            getCategoriesListRequest(this.queryInfo).then(res =>{
                let result =res.data;

                if(result.meta.status !== 200){
                    return this.alertMessage('商品列表获取失败','error')
                }

                this.category = result.data.result;
                this.total = result.data.total
            })
        },

        //监听子组件触发的事件
        updateCategoryList(){
            this.getCategoriesList();
        },

        //监听pagesize改变
        handleSizeChange(newSize){
            this.queryInfo.pagesize = newSize;
            this.getCategoriesList();
        },

        handleCurrentChange(newPage){
            this.queryInfo.pagenum = newPage;
            this.getCategoriesList();
        },
    }
}
</script>

<style>

</style>