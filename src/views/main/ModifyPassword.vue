<template>
    <div shadow="hover" style="display: flex; justify-content:center;">
        <el-card style="width: 480px;">
            <el-form ref="modifyPasswordRef" :rules="rules" :model="modifyPassword" label-width="80px">
                <el-form-item label="原始密码" prop="oldPassword">
                    <el-input v-model="modifyPassword.oldPassword" show-password placeholder="请输入原始密码"></el-input>
                </el-form-item>
                <el-form-item label="新密码" prop="newPassword">
                    <el-input v-model="modifyPassword.newPassword" show-password placeholder="请输入新密码"></el-input>
                </el-form-item>
                <el-form-item label="确认密码" prop="confirmPassword">
                    <el-input v-model="modifyPassword.confirmPassword" show-password placeholder="请确认密码"></el-input>
                </el-form-item>
            </el-form>
            <div style="display: flex; justify-content: center; align-items: center;">
                <el-button type="primary" @click="confirm">确定</el-button>
                <el-button type="info" @click="cancel">取消</el-button>
            </div>
        </el-card>
    </div>
</template>

<script>
export default {
    data() {
        var validConfirmPasswordRule = (rule, value, callback) => {
            if (value === '') {
                callback(new Error('请确认密码！'));
            } else if (this.modifyPassword.newPassword != value) {
                callback(new Error('密码不一致！'));
            } else {
                callback();
            }
        };

        return {
            modifyPassword: {
                oldPassword: '',
                newPassword: '',
                confirmPassword: ''
            },
            rules: {
                oldPassword: [
                    { required: true, message: '请输入原密码', trigger: 'blur' },
                    { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
                ],
                newPassword: [
                    { required: true, message: '请输入新密码', trigger: 'blur' },
                    { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
                ],
                confirmPassword: [
                    { validator: validConfirmPasswordRule, trigger: 'blur' }
                ]
            }
        }
    },
    //页面加载好后触发的操作
    mounted() {

    },

    methods: {
        confirm() {
            this.$refs['modifyPasswordRef'].validate((valid) => {
                if (valid) {
                    this.request.post("user/updatePassword", this.modifyPassword).then(response => {
                        console.log(response)
                        if (response.code != '200') {
                            this.$message.error("修改用户密码失败！")
                        } else {
                            //修改密码不更新token
                            //localStorage.setItem("token", JSON.stringify(response.data.token))
                            this.$router.push("/");
                        }
                    })
                }
            })
        },
        cancel() {
            this.$router.push("/");
        }
    }
}
</script>

<style></style>