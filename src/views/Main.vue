
import router from '../router';

<template>
    <div>
        <el-container>
            <el-aside :width="asideWidth" style="min-height: 100vh; background-color: #001529;">
               <div style="height: 60px; line-height: 60px; color: white; display: flex; align-items: center;justify-content: center">
                    <img src="@/assets/moyu_logo.jpg" style="width: 40px;height: 40px;">
                    <Transition name="el-fade-in-linear">
                    <!-- <span style="margin-left: 5px;font-size: 18px;" v-show="!isCollapse">摸鱼咯</span> -->
                    </Transition>
                </div>

               <el-menu :collapse-transition="false" :collapse="isCollapse" router background-color="#001529" text-color="rgba(255,255,255,0.65)" active-text-color="#fff" style="border: none; " :default-active="$route.path">
                    <el-menu-item index="/home">               
                        <i class="el-icon-house"></i>
                        <span slot="title">摸鱼热榜</span>                        
                    </el-menu-item>
                    <el-menu-item index="/goCrazy">                        
                        <i class="el-icon-s-data"></i>
                        <span slot="title">我要发疯</span>                       
                    </el-menu-item>

                    <el-menu-item index="/roast">                        
                        <i class="el-icon-s-data"></i>
                        <span slot="title">吐槽我们</span>                       
                    </el-menu-item>
               </el-menu>

            </el-aside>

            <el-container>
                <el-header>
                    <!-- 面包屑 -->
                    <i :class="collapseIcon" style="font-size: 16px;" @click="handleCollapse"></i>
                    <el-breadcrumb separator-class="el-icon-arrow-right" style="margin-left: 20px;">
                    
                        <el-breadcrumb-item :to="{ path: $route.path}">{{ $route.name}}</el-breadcrumb-item>
                    </el-breadcrumb>

                    <!-- 用户信息 -->
                    <!-- <div style="flex: 1; width: 0; display: flex; align-items: center; justify-content: flex-end;">
                        <el-dropdown placement="bottom">
                            <div style="display: flex; align-items: center; cursor: default;">
                                <img :src="headPic" style="width: 40px; height: 40px;">
                                <span v-text="headUserName"></span>
                            </div>                       
                            <el-dropdown-menu slot="dropdown" >
                                <el-dropdown-item @click.native="personInfo">个人信息</el-dropdown-item>
                                <el-dropdown-item @click.native="modifyPassword">修改密码</el-dropdown-item>
                                <el-dropdown-item @click.native="logout">退出登录</el-dropdown-item>                       
                            </el-dropdown-menu>
                        </el-dropdown>
                    </div> -->
                </el-header>

                <el-main>
                    <router-view />
                </el-main>
            </el-container>

        </el-container>
    </div>
</template>


<script>

import axios from 'axios';  

export default {
    data() {
        return {
            isCollapse : false,
            asideWidth : '200px',
            collapseIcon : 'el-icon-s-fold',
            user: {
                id: '1',
                birthday: '2016-05-02',
                username: '王小虎',
                phone: '13627456398',
                address: '上海市普陀区金沙江路 1518 弄',
                mail: 'qq@qq.com',
                headPic : '@/assets/logo.jpg',
            },
            headPic : '@/assets/logo.jpg',
            headUserName: '王小虎'
        }
    },
    //页面加载好后触发的操作
    mounted() {
        console.log(this.$route)
    },
    //页面创建时触发的
    created() {
        let token = localStorage.getItem("token") ;
        let userInfo = JSON.parse(token);
        this.user = userInfo;
        this.headUserName = userInfo.username;
        this.headPic = userInfo.headPic;
    },
    methods: {
        handleCollapse() {
            this.isCollapse = !this.isCollapse
            this.asideWidth = this.isCollapse ? '64px' : '200px'
            this.collapseIcon = this.isCollapse ? 'el-icon-s-unfold' : 'el-icon-s-fold'  
        },
        logout() {
            localStorage.removeItem("token");
            this.$router.push("/login");
        },
        modifyPassword() {
            this.$router.push("/modifyPassword")
        },
        personInfo() {
            this.$router.push("/person")
        }
    }
}
</script>

<style>
.el-menu--inline {
    background-color: #000c17 !important;
}
.el-menu--inline .el-menu-item {
    background-color: #000c17 !important;
    padding-left: 49px !important;
}
.el-menu-item:hover, .el-submenu__title:hover {
    color: #fff !important;
}
.el-submenu__title:hover i {
    color: #fff !important;
}
.el-menu-item.is-active {
    background-color: #1890ff !important;
    border-radius: 5px !important;
}
.el-menu-item {
    height: 40px !important;
    line-height: 40px !important;
}
.el-submenu__title {
    height: 40px !important;
    line-height: 40px !important;
    padding: 0 60px 0 0;
}
.el-submenu__icon-arrow {
    margin-top: -5px;
}
.el-aside {
    transition: width .3s;
    box-shadow: 2px 0 6px rgba(0, 21, 41, 35);
}
.el-header {
    box-shadow: 2px 0 6px rgba(0, 21, 41, 35);
    display: flex;
    align-items: center;
}
.el-dropdown-menu__item, .el-menu-item{
    padding: 0 40px 0 0  !important;
}
.el-menu--inline .el-menu-item {
    padding-left: 75px !important;
    text-align: left;
}

</style>