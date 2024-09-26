<template>
    <div style="height: 100vh; display: flex; justify-content: center; align-items: center; background-color: #669fef;">
        <div style="display: flex; background-color: white; width: 50%; border-radius: 5px; overflow: hidden;">
            <div style="flex: 1;">
                <img src="@/assets/register.jpg" alt="" style="width: 100%;">
            </div>

            <div style="flex: 1; display: flex; align-items: center; justify-content: center;">
                <el-form ref="userRef" :rules="rules" :model="user" style="width: 80%;">
                    <div style="font-size: 20px; font-weight: bold; text-align: center; margin-bottom: 30px;">欢迎注册xxx系统</div>
                    
                    <el-form-item prop="username">
                        <el-input prefix-icon="el-icon-user" v-model="user.username" style="font-size: 12px;" placeholder="请输入用户名"></el-input>
                    </el-form-item>
                    <el-form-item prop="password">
                        <el-input show-password prefix-icon="el-icon-lock" v-model="user.password" style="font-size: 12px;" placeholder="请输入密码"></el-input>
                    </el-form-item>
                    <el-form-item prop="confirmPassword">
                        <el-input show-password prefix-icon="el-icon-lock" v-model="user.confirmPassword" style="font-size: 12px;" placeholder="请确认密码"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" style="width: 100%;" @click="login">注册</el-button>
                    </el-form-item>
                    <div style="text-align: left; font-size: 15px;">
                            已有账号？去<span style="color: #0f9876; cursor: pointer;" @click="$router.push('/login')">登录</span> 
                    </div>
                </el-form>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        name: 'Login',
        components:{
            
        },
        data() {
            var validConfirmPasswordRule = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请确认密码！'));
                } else if(this.user.password != value) {            
                    callback(new Error('密码不一致！'));                  
                } else {
                    callback();
                }
            };

            return {
                user : {
                    username : '',
                    password : '',
                    confirmPassword: ''
                },

                rules: {
                    username: [
                        { required: true, message: '请输入用户名', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'blur' },
                        { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
                    ],
                    confirmPassword: [
                        { validator: validConfirmPasswordRule, trigger: 'blur' }
                    ],
                }
            }
        
        },
        methods: {
            login() {
                this.$refs['userRef'].validate((valid) => {
                    if (valid) {
                        this.request.post("user/register", this.user).then(response => {
                            console.log(response)
                            if (response.code != '200') {
                                this.$message.error(response.message)
                            } else {
                                this.$message({
                                    message: '注册成功！',
                                    type: 'success'
                                });

                                //此处需要服务端查询下注册的用户信息返回存入浏览器
                                localStorage.setItem("token", JSON.stringify(response.data));
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