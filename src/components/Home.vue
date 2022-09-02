<template>
  
</template>

<script>
import { getMenuListRequest } from '@/network/home,'
export default {
name:'Home',
data(){
    return {
        menuList:[],
        icons:['iconfont icon-users', 'iconfont icon-tijikongjian', 'iconfont icon-shangpin', 'iconfont icon-danju', 'iconfont icon-baobiao'],
        isCollapse: false,
        activePath:'',//被激活的链接地址
    }
},
created(){
    this.getMenuList()
    let activePath = sessionStorage.getItem('activePath');
    this.activePath = activePath;
},
methods: {
    loginOut(){
    sessionStorage.removeItem('token');
    this.$router.replace('/login');
    },
    getMenuList(){
        getMenuListRequest().then(res => {
            let result = res.data;
            if (result.meta.status !== 200){
                this.$message({
                    message: result.meta.message,
                    type:'error'
                })
            }
            this.menuList = result.data;
        })
    },
    //点击按钮切换左侧菜单的折叠与展开
    toggleCollapse(){
        this.isCollapse = !this.isCollapse
    },
    //保存当前激活的路径
    saveNavState(activePath){
        sessionStorage.setItem('activePath',activePath);
        this.activePath = activePath;
        }
    }
}
</script>

<style scoped>
  .home-container {
    height: 100%;
  }

  .el-header {
    background-color: rgb(54, 61, 64);
    color: white;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 20px;
  }

  .header-logo {
    display: flex;
    align-items: center;
  }

  .header-logo img {
    height: 50px;
  }

  /* 左侧菜单 */
  .el-aside {
    background-color: rgb(49, 55, 67);
    color: white;
    user-select: none;
    transition: width 0.3s;
  }

  .iconfont {
    margin-right: 10px;
  }

  .el-menu {
    border-right: none;
  }

  .toggle-button {
    background-color: #4A5064;
    font-size: 14px;
    text-align: center;
    line-height: 30px;
    letter-spacing: 2px;
    cursor: pointer;
  }

  /* 右侧内容区 */
  .el-main {
    background-color: rgb(233, 237, 241);
  }

</style>

