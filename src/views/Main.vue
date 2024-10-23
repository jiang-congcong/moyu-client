import router from '../router';

<template>
    <div>
        <el-container>
            <el-aside :width="asideWidth" style="min-height: 100vh; background-color: #001529;">
                <div
                    style="height: 60px; line-height: 60px; color: white; display: flex; align-items: center;justify-content: center">
                    <img src="@/assets/moyu_logo.jpg" style="width: 40px;height: 40px;">
                    <Transition name="el-fade-in-linear">
                        <!-- <span style="margin-left: 5px;font-size: 18px;" v-show="!isCollapse">摸鱼咯</span> -->
                    </Transition>
                </div>

                <el-menu :collapse-transition="false" :collapse="isCollapse" router background-color="#001529"
                    text-color="rgba(255,255,255,0.65)" active-text-color="#fff" style="border: none; "
                    :default-active="$route.path">
                    <el-menu-item index="/home">
                        <i class="el-icon-house"></i>
                        <span slot="title">摸鱼热榜</span>
                    </el-menu-item>
                    <el-menu-item index="/goCrazy">
                        <i class="el-icon-s-data"></i>
                        <span slot="title">我要发疯</span>
                    </el-menu-item>

                    <el-menu-item index="/roast">
                        <i class="el-icon-user-solid"></i>
                        <span slot="title">吐槽我们</span>
                    </el-menu-item>
                </el-menu>

            </el-aside>

            <el-container>
                <el-header>
                    <!-- 面包屑 -->
                    <i :class="collapseIcon" style="font-size: 16px;" @click="handleCollapse"></i>
                    <el-breadcrumb separator-class="el-icon-arrow-right" style="margin-left: 20px;">

                        <el-breadcrumb-item :to="{ path: $route.path }">{{ $route.name }}</el-breadcrumb-item>
                    </el-breadcrumb>

                    <!-- 用户信息 -->
                    <div style="flex: 1; display: flex; align-items: center; justify-content: flex-end;">
                        <img :src="headPic" style="width: 40px; height: 40px; border-radius: 30px; margin-right: 10px;">
                        <el-dropdown :hide-on-click="false">
                            <span class="el-dropdown-link">
                                {{ headUserName }}<i class="el-icon-arrow-down el-icon--right"></i>
                            </span>
                            <el-dropdown-menu slot="dropdown">
                                <el-dropdown-item icon="el-icon-user" @click.native="queryUserInfo" id="userInfoClass"
                                    style="">个人信息</el-dropdown-item>
                                <el-dropdown-item icon="el-icon-edit" @click.native="editPassword"
                                    id="editPasswordClass" style="">修改密码</el-dropdown-item>
                                <el-dropdown-item icon="el-icon-share" @click.native="logoutOrLogin">{{ loginStateName
                                    }}</el-dropdown-item>
                            </el-dropdown-menu>
                        </el-dropdown>
                    </div>
                </el-header>

                <el-main>
                    <router-view />
                </el-main>
            </el-container>

        </el-container>

        <!-- 登录 -->
        <el-dialog :visible.sync="loginDialogVisible" width="30%">
            <div style="display: flex; align-items: center; justify-content: center;">
                <el-form ref="userRef" :rules="rules" :model="user" style="width: 80%;">
                    <div style="font-size: 20px; font-weight: bold; text-align: center; margin-bottom: 30px;">欢迎登陆我要摸鱼系统
                    </div>

                    <el-form-item prop="username">
                        <el-input prefix-icon="el-icon-user" v-model="user.username" style="font-size: 12px;"
                            placeholder="请输入用户名"></el-input>
                    </el-form-item>
                
                    <el-form-item prop="password">
                        <el-input show-password prefix-icon="el-icon-lock" v-model="user.password"
                            style="font-size: 12px;" placeholder="请输入密码"></el-input>
                    </el-form-item>
                    <el-form-item prop="validCodeInput">
                        <div style="display: flex;">
                            <el-input prefix-icon="el-icon-circle-check" v-model="user.validCodeInput" size="medium"
                                style="flex: 1; font-size: 12px;" placeholder="请输入验证码"></el-input>
                            <div style="flex: 1; height: 34px;">
                                <valid-code @update:value="getCode"></valid-code>
                            </div>
                        </div>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" style="width: 100%;" @click="login">登录</el-button>
                    </el-form-item>
                    <div style="display:flex; font-size: 13px;">
                        <div style="flex: 1; text-align: left;">
                            还没有账号？请<span style="color: #0f9876; cursor: pointer;"
                                @click="goRegister">注册</span>
                        </div>
                        <div style="flex: 1; text-align: right;color: #0f9876; cursor: pointer;">
                            忘记密码
                        </div>
                    </div>
                </el-form>
            </div>

        </el-dialog>

    </div>
</template>


<script>

import axios from 'axios';
import ValidCode from '@/components/ValidCode.vue';

export default {
    components: {
        ValidCode   //here
    },
    data() {
        var validCodeRule = (rule, value, callback) => {
            if (value === '') {
                callback(new Error('请输入验证码'));
            } else if (this.validCodeComponent.toLowerCase() !== value.toLowerCase()) {
                callback(new Error('验证码错误！'));
            } else {
                callback();
            }
        };

        return {
            validCodeComponent: '', //验证码组件的值

            rules: {
                username: [
                    { required: true, message: '请输入用户名', trigger: 'blur' }
                ],
                
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur' },
                    { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
                ],
                validCodeInput: [
                    { validator: validCodeRule, trigger: 'blur' }
                ],
                
            },

            isCollapse: false,
            asideWidth: '200px',
            collapseIcon: 'el-icon-s-fold',
            user: {
                id: '',
                password: '',
                username: '',
                phone: '',
                address: '',
                mail: '',
                confirmPassword: ''
            },
            headPic: 'http://1.94.107.192:8084/images/profile.jpg',
            headUserName: '摸鱼人',
            loginStateName: "登录",
            loginDialogVisible: false
        }
    },
    //页面加载好后触发的操作
    mounted() {
        let token = localStorage.getItem("moyu_token");
        if (token == null) {
            document.querySelector('#userInfoClass').style.display = 'none';
            document.querySelector('#editPasswordClass').style.display = 'none';
            this.loginStateName = '登录'
            
        } else {
            let parseToken = JSON.parse(token)
            document.querySelector('#userInfoClass').style.display = 'block';
            document.querySelector('#editPasswordClass').style.display = 'block';
            this.loginStateName = '退出登录'
            this.headPic = parseToken.headImageUrl
            this.headUserName = parseToken.username
            this.user = response.data
        };



    },
    //页面创建时触发的
    created() {
    },
    methods: {
        getCode(code) {
            this.validCodeComponent = code;
            console.log("验证码组件code:" + code);
        },
        handleCollapse() {
            this.isCollapse = !this.isCollapse
            this.asideWidth = this.isCollapse ? '64px' : '200px'
            this.collapseIcon = this.isCollapse ? 'el-icon-s-unfold' : 'el-icon-s-fold'
        },
        logoutOrLogin() {
            if (this.loginStateName === '登录') {
                this.loginDialogVisible = true
            } else {
                document.querySelector('#userInfoClass').style.display = 'none';
                document.querySelector('#editPasswordClass').style.display = 'none';
                this.loginStateName = '登录'
                localStorage.removeItem('moyu_token')
                this.headUserName = '摸鱼人',
                this.headPic = 'http://1.94.107.192:8084/images/profile.jpg'
            }
        },
        modifyPassword() {
            this.$router.push("/modifyPassword")
        },
        personInfo() {
            this.$router.push("/person")
        },
        goRegister() {
            this.loginDialogVisible = false;
            this.$router.push("/register1")
        },
        login() {
            this.$refs['userRef'].validate((valid) => {
                if (valid) {
                    this.request.post("user/login", this.user).then(response => {                       
                        if (response.code != '200') {
                            this.$message.error(response.message)
                        } else {
                            this.loginDialogVisible = false;
                            document.querySelector('#userInfoClass').style.display = 'block';
                            document.querySelector('#editPasswordClass').style.display = 'block';
                            this.loginStateName = '退出登录';
                        
                            this.headUserName = response.data.username,
                            this.headPic = response.data.headImageUrl
                            this.user = response.data
                            // 用户信息存入localstorage
                            let respData = response.data
                            localStorage.setItem("moyu_token", JSON.stringify(respData))
                        }
                    })
                }
            });
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

.el-menu-item:hover,
.el-submenu__title:hover {
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

/* .el-dropdown-menu__item,
.el-menu-item {
    padding: 0 40px 0 0 !important;
} */

.el-menu--inline .el-menu-item {
    padding-left: 75px !important;
    text-align: left;
}

.el-dropdown-link {
    cursor: pointer;
    color: #409EFF;
}

.el-icon-arrow-down {
    font-size: 12px;
}
</style>