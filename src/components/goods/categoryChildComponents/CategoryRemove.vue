<template>
  <el-button type="danger" size="mini" icon="el-icon-delete" @click="removeCategory(id)">删除</el-button>

</template>

<script>
import  {removeCategoryByIdRequest} from 'network/goods';
export default {
    name:'CategoryRemove',
    props:{
        id:{
            type:Number,
            default:0
        }
    },
    methods:{
        removeCategory(id){
            this.$confirm()
        }
    }
}
</script>

<style>

</style>