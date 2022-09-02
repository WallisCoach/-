<template>
  <div>
    <el-switch v-model="nowState" @change="userStateChange(userInfo)">
    
    </el-switch>
  </div>
</template>

<script>
import {changUserStateRequest} from 'network/user';
export default {
    name:'UserSetState',
    props:{
        state:{
            type:Boolean,
            required :true
        },
        userInfo:{
            type:Object,
            default(){}
        }
    },
    data(){
        return {
            nowState:this.state
        }
    },
    methods:{
        userStateChange(userInfo){
            changUserStateRequest(userInfo.id,this.nowState).then(res => {
                let result = res.data;
                if(result.meta.status) {
                    this.nowState = !this.nowState
                    return this.alertMessage('更新状态失败','error');
                }
                this.alertMessage('更新状态成功','success');     
            })
        }
    }
}
</script>

<style>

</style>