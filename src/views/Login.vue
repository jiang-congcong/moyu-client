<template>
    <div style="height: 100vh; display: flex; justify-content: center; align-items: center; background-color: aquamarine;">
        <div style="display: flex; background-color: white; width: 50%; border-radius: 5px; overflow: hidden;">
            <div style="flex: 1;">
                <img src="@/assets/login.jpg" alt="" style="width: 100%;">
            </div>

            <div style="flex: 1; display: flex; align-items: center; justify-content: center;">
                <el-form ref="userRef" :rules="rules" :model="user" style="width: 80%;">
                    <div style="font-size: 20px; font-weight: bold; text-align: center; margin-bottom: 30px;">欢迎登陆xxx系统</div>
                    
                    <el-form-item prop="username">
                        <el-input prefix-icon="el-icon-user" v-model="user.username" style="font-size: 12px;" placeholder="请输入用户名"></el-input>
                    </el-form-item>
                    <el-form-item prop="password">
                        <el-input show-password prefix-icon="el-icon-lock" v-model="user.password" style="font-size: 12px;" placeholder="请输入密码"></el-input>
                    </el-form-item>
                    <el-form-item prop="validCodeInput">
                        <div style="display: flex;">
                            <el-input prefix-icon="el-icon-circle-check" v-model="user.validCodeInput" size="medium" style="flex: 1; font-size: 12px;" placeholder="请输入验证码"></el-input>
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
                            还没有账号？请<span style="color: #0f9876; cursor: pointer;" @click="$router.push('/register')">注册</span>
                        </div>
                        <div style="flex: 1; text-align: right;color: #0f9876; cursor: pointer;">
                            忘记密码
                        </div>
                    </div>
                </el-form>
            </div>
        </div>
    </div>
</template>

<script>
import ValidCode from '@/components/ValidCode.vue';

    export default {
        name: 'Login',
        components:{
            ValidCode   //here
        },
        data() {
            var validCodeRule = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入验证码'));
                } else if(this.validCodeComponent.toLowerCase() !== value.toLowerCase()) {            
                    callback(new Error('验证码错误！'));                  
                } else {
                    callback();
                }
            };

            return {
                user : {
                    username : '',
                    password : '',
                    validCodeInput: '' //验证码输入框输入的值
                },
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
                }
            }
        
        },
        methods: {
            getCode(code) {
                this.validCodeComponent = code;
                console.log("验证码组件code:" + code);
            },

            login() {
                this.$refs['userRef'].validate((valid) => {
                    if (valid) {
                        this.request.post("user/login", this.user).then(response => {
                            console.log(response)
                            if (response.code != '200') {
                                this.$message.error("用户名或密码错误！")
                            } else {
                                localStorage.setItem("token", JSON.stringify(response.data))
                                this.$router.push("/");
                            }
                        })
                    }
                })
            
            }
        }
    }
</script>

<style>

</style>