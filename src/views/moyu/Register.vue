<template>
    <div style="display: flex; justify-content: center; align-items: center;">
        <div style="display: flex; align-items: center; justify-content: center; width: 40%;">
            <el-form ref="userRef" :rules="rules" :model="user" style="width: 80%;">
                <div style="font-size: 20px; font-weight: bold; text-align: center; margin-bottom: 30px;">欢迎注册我要摸鱼系统
                </div>

                <el-upload class="avatar-uploader" action="http://1.94.107.192:8085/user/upload" :show-file-list="false" :on-success="handleAvatarSuccess"
                    :before-upload="beforeAvatarUpload">
                    <img v-if="imageUrl" :src="imageUrl" class="avatar">
                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                    <p style="font-size: 15px;">头像</p>
                </el-upload>
            
                <el-form-item prop="username">
                    <el-input prefix-icon="el-icon-user" v-model="user.username" style="font-size: 12px;"
                        placeholder="请输入用户名"></el-input>
                </el-form-item>
                <el-form-item prop="mail">
                    <el-input prefix-icon="el-icon-s-promotion" v-model="user.mail" style="font-size: 12px;"
                        placeholder="请输入邮箱"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input show-password prefix-icon="el-icon-lock" v-model="user.password" style="font-size: 12px;"
                        placeholder="请输入密码"></el-input>
                </el-form-item>
                <el-form-item prop="confirmPassword">
                    <el-input show-password prefix-icon="el-icon-lock" v-model="user.confirmPassword"
                        style="font-size: 12px;" placeholder="请确认密码"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" style="width: 100%;" @click="register">注册</el-button>
                </el-form-item>
                <!-- <div style="text-align: left; font-size: 15px;">
                    已有账号？去<span style="color: #0f9876; cursor: pointer;" @click="$router.push('/login')">登录</span>
                </div> -->
            </el-form>
        </div>
    </div>

</template>

<script>
export default {
    name: 'Register',
    components: {

    },
    data() {
        var validConfirmPasswordRule = (rule, value, callback) => {
            if (value === '') {
                callback(new Error('请确认密码！'));
            } else if (this.user.password != value) {
                callback(new Error('密码不一致！'));
            } else {
                callback();
            }
        };

        return {
            user: {
                username: '',
                mail: '',
                password: '',
                confirmPassword: '',
                headImageUrl: ''
            },

            rules: {
                username: [
                    { required: true, message: '请输入用户名', trigger: 'blur' }
                ],
                mail: [
                    { required: true, message: '请输入邮箱', trigger: 'blur' }
                ],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur' },
                    { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
                ],
                confirmPassword: [
                    { validator: validConfirmPasswordRule, trigger: 'blur' }
                ],
            },

            imageUrl: ''
        }

    },
    methods: {
        register() {
            this.$refs['userRef'].validate((valid) => {
                if (valid) {
                    this.request.post("user/register", this.user).then(response => {
                        if (response.code != '200') {
                            this.$message.error(response.message)
                        } else {
                            this.$message({
                                message: '注册成功！',
                                type: 'success'
                            });

                            //此处需要服务端查询下注册的用户信息返回存入浏览器
                            localStorage.setItem("moyu_token", JSON.stringify(response.data));
                            window.location.replace("/");
                        }
                    })
                }
            })

        },

        handleAvatarSuccess(res, file) {
            console.log(res)
            console.log(file)
            this.imageUrl = URL.createObjectURL(file.raw);
            this.user.headImageUrl = 'http://1.94.107.192:8084/' + res.data
        },
        beforeAvatarUpload(file) {
            console.log(file)
            const isJPG = file.type === 'image/jpeg';
            const isLt2M = file.size / 1024 / 1024 < 2;

            if (!isJPG) {
                this.$message.error('上传头像图片只能是 JPG 格式!');
            }
            if (!isLt2M) {
                this.$message.error('上传头像图片大小不能超过 2MB!');
            }
            return isJPG && isLt2M;
        }
    }
}
</script>

<style scoped>
.avatar-uploader .el-upload {
    border: 1px dashed #8c939d ;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
}

.avatar-uploader .el-upload:hover {
    border-color: #409EFF;
}

.avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
    border: 1px dashed #8c939d ;
}

.avatar {
    width: 178px;
    height: 178px;
    display: block;
}
</style>