<template>
    <div style="display: flex; justify-content: center; align-items: center;">
        <div style="display: flex; align-items: center; justify-content: center; width: 40%;">
            <el-form ref="userRef" :rules="rules" :model="user" style="width: 80%;">
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
                    :disabled="true"></el-input>
                </el-form-item>
                
                <el-form-item>
                    <el-button type="primary" style="width: 100%;" @click="updateUserInfo">修改</el-button>
                </el-form-item>
                
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

        return {
            user: {
                username: '',
                mail: '',
                headImageUrl: ''
            },

            rules: {
                username: [
                    { required: true, message: '请输入用户名', trigger: 'blur' }
                ],
            },

            imageUrl: ''
        }

    },

    //页面加载好后触发的操作
    mounted() {
        let token = localStorage.getItem("moyu_token");
        let parseToken = JSON.parse(token)

        this.imageUrl = parseToken.headImageUrl
        this.user = parseToken
    },
    //页面创建时触发的
    created() {

    },

    methods: {
        updateUserInfo() {
            this.$refs['userRef'].validate((valid) => {
                if (valid) {
                    this.request.post("user/updateUserInfo", {'id': this.user.id, 'username': this.user.username, 'headImageUrl': this.imageUrl}).then(response => {
                        if (response.code != '200') {
                            this.$message.error(response.message)
                        } else {
                            this.$message({
                                message: '修改用户信息成功！',
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
            this.imageUrl = URL.createObjectURL(file.raw);
            this.user.headImageUrl = 'http://1.94.107.192:8084/' + res.data
        },
        beforeAvatarUpload(file) {
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