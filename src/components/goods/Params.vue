<template>
  <div>
    <breadcrumb-nav>
    <template v-slot:firstMenu>商品管理</template>
    <template v-slot:secondMenu>分类参数</template>
    </breadcrumb-nav>

    <!-- 卡片视图 -->
    <el-card>
        <!-- 头部警告区域 -->
        <el-alert title="注意：只允许第三级分类设置相关参数" type="warning" :closeable="false" show-icon></el-alert>

        <!-- 选择商品分类区域 -->
        <el-row class="selectCategory">
            <el-col>
                <span>选择商品分类：</span>
                <!-- 选择商品分类的级联选择框 -->
                <el-cascader
                v-model="selectedKeys"
                :options="categoryList"
                :props="cascaderProps"
                @change="categoryChange">
                </el-cascader>
            </el-col>
        </el-row>
        <!--标签页 -->
        <el-tabs v-model="activeName" @tab-click="ahndleTabClick">
        <el-tab-pane label="动态参数" name="many">
            <param-tab-pane
            :table-data="manyTableData"
            :title-text="titleText"
            :is-btn-disabled="isBtnDisabled"
            :category-id="categoryId"
            :active-name="activeName"
            @param-list="updateParamsList">
            </param-tab-pane>
        </el-tab-pane>

        <el-tab-pane label="静态属性" name="only">
            <param-tab-pane
                    :table-data="onlyTableData"
                    :title-text="titleText"
                    :is-btn-disabled="isBtnDisabled"
                    :category-id="categoryId"
                    :active-name="activeName"
                    @param-list="updateParamsList">
            </param-tab-pane>
        </el-tab-pane>
        </el-tabs>
    </el-card>
  </div>
</template>

<script>
import BreadcrumbNav from '../common/BreadcrumbNav.vue';
import ParamTbaPane from ''
import {getCategoriesListRequest,getCategoryParamsRequest} from ''
export default {
    name:'Params',
    components:{
        BreadcrumbNav,
        ParamTbaPane
    },
    data(){
        return {
            categoryList:[],
            cascaderProps:{
                expandTrigger:'hover',
                value:'cat_id',
                label:'cat_name',
                children:'children',
                checkStrictly:true
            },
            selectedKeys:[],//级联选择框中选中项的数组
            activeName:'many',
            manyTableData:[],
            onlyTableData:[],
        }
    },
    computed:{
        created(){
            this.getCategoriesList();
        },
        categoryId(){
            if(this.selectedKeys.length === 3){
                return this.selectedKeys[2];
            }
            return null;
        },
        titleText(){
            return this.activeName ==='many'?'动态参数' :'静态属性';
        },
    },
    methods:{
        getCategoriesList(){
            getCategoriesListRequest(this.queryInfo).then(res => {
                let result = res.data;
                if(result.meta.status !== 200){
                    return this.alertMessage('商品列表获取失败','error');
                }

                this.categoryList = result.data;
            })
        },

        //选择项发送变化触发的函数
        categoryChange(){
            let selectedKeys = this.selectedKeys;
            console.log(selectedKeys);
        // 选中的不是三级分类，清空选择项
        if (selectedKeys.length < 3) {
          this.selectedKeys = [];
          return;
        }
        this.getCategoryParams();
        },

        //tab标签页点击事件的处理函数
        handleTabClick(){
            console.log(this.activeName);
            this.getCategoryParams();
        },

        //获取参数列表
        getCategoryParmas(){
            getCategoryParamsRequest(this.categoryId,this.activeName).then(res => {
            let result = res.data;
            console.log('参数:',result);
            if (result.meta.status !== 200){
                return this.alertMessage('获取参数失败','error');
            }

            if (this.activeName ==='many'){
                this.manyTableData = result.data;
            } else {
                this.onlyTableData = result.data;
            }
            })
        },

        //监听子组件触发的事件
        updataParamsList(){
            this.getCategoryParams();
        }
    }
}

</script>

<style>
.selectCategory{
    margin-top:15px;
}
</style>