<template>

    <el-container class="home-container">
        <!-- 头部区域 -->
        <el-header>
            <div>
                <img src="../assets/logo.png" alt="">
                <span>用户后台管理系统</span>
            </div>
            <el-button type="info" @click="logout">退出</el-button>
        </el-header>
        <!-- 页面主体 -->
        <el-container>
            <!-- 左侧边栏 -->
            <el-aside :width="isCollapse ? '64px' : '200px' ">
                <div class="toggle-button" @click="toggleCollapse">|||</div>

                <!-- 侧边栏菜单区域 -->
                <el-menu class="el-menu-vertical-demo" background-color="#545c64" text-color="#fff" active-text-color="#ffd04b" 
                unique-opened :collapse="isCollapse" :collapse-transition="false" router :default-active="activePath">

                    <!-- 一级菜单 -->
                    <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">

                        <!-- 一级菜单模板区 -->
                        <template slot="title">

                            <!-- 模板区图标 -->
                            <i :class="iconsObj[item.id]"></i>

                            <!-- 模板区文本 -->
                            <span>{{item.authName}}</span>
                        </template>

                        <!-- 二级菜单 -->
                        <el-menu-item :index=" '/' + subItem.path + ''" v-for="subItem in item.children" 
                        :key="subItem.id" @click="saveNavState('/' + subItem.path)">
                            <template slot="title">
                                <!-- 模板区图标 -->
                                <i class="el-icon-menu"></i>

                                <!-- 模板区文本 -->
                                <span>{{subItem.authName}}</span>
                            </template>
                        </el-menu-item>
                    </el-submenu>                   
                </el-menu>
            </el-aside>
            <!-- 右侧主体 -->
            <el-main>
                <!-- 在这里放置一个路由占位符 -->
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>

</template>

<script>
export default {
    data () {
        // 左侧菜单数据
        return {
            menulist:[],
            iconsObj:{
                125:'iconfont el-icon-user-solid',
                103:'iconfont el-icon-s-grid',
                101:'iconfont el-icon-s-goods',
                102:'iconfont el-icon-s-order',
                145:'iconfont el-icon-data-line'
            },
            // 是否进行折叠
            isCollapse:false,
            // 被激活的链接地址
            activePath:'',
        }
    },
    created () {
        this.getMenuList()
        this.activePath = window.sessionStorage.getItem('activePath')
    },
    methods: {
        logout(){
            window.sessionStorage.clear();
            this.$router.push('/login');
        },
        async getMenuList(){
            const {data:res} = await this.$http.get('menus')
            if(res.meta.status !==200) return this.$message.error(res.meta.msg)
            this.menulist=res.data
            console.log(res);
        },
        // 点击按钮，切换菜单的折叠与展开
        toggleCollapse(){
            this.isCollapse = !this.isCollapse
        },
        saveNavState(activePath){
            window.sessionStorage.setItem('activePath',activePath)
            this.activePath = activePath
        }
    }
   
};
</script>

<style>
.home-container{
    height: 100%;
}
.el-header{
    background-color: #242729;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.el-aside>.el-menu{
    border-right: none;
}
.el-header>div{
    display: flex;
    align-items: center;
}
.el-header>div>span{
    margin-left: 30px;
    color: aliceblue;
    font-size: 20px;
}
.el-header>div>img{
    width: 65px;
    height: 65px;
    margin-left: 30px;
}
.el-aside{
    background-color: #363e5a;
}
.el-main{
    background-color: #eaedf1;
}
.iconfont{
    margin-right: 10px;
}
.toggle-button{
    height: 30px;
    background-color: #4A5064;
    color: aliceblue;
    text-align: center;
    font-size: 14px;
    line-height: 30px;
    cursor: pointer;
    letter-spacing: 0.2em;
}
</style>