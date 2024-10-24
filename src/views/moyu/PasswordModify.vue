<template>
    <div style="display: flex; justify-content: center; align-items: center;">
        <div style="display: flex; align-items: center; justify-content: center; width: 40%;">
            <el-form ref="userRef" :rules="rules" :model="user" style="width: 80%;">
                <el-form-item prop="oldPassword">
                    <el-input show-password prefix-icon="el-icon-lock" v-model="user.oldPassword" style="font-size: 12px;"
                        placeholder="请输入原密码"></el-input>
                </el-form-item>
                <el-form-item prop="newPassword">
                    <el-input show-password prefix-icon="el-icon-lock" v-model="user.newPassword" style="font-size: 12px;"
                        placeholder="请输入新密码"></el-input>
                </el-form-item>
                <el-form-item prop="confirmPassword">
                    <el-input show-password prefix-icon="el-icon-lock" v-model="user.confirmPassword"
                        style="font-size: 12px;" placeholder="请确认新密码"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" style="width: 100%;" @click="updatePassword">修改</el-button>
                </el-form-item>
                <div style="text-align: right; font-size: 15px;">
                    忘记密码？去<span style="color: #0f9876; cursor: pointer;" @click="openResetDialog">重置</span>
                </div>
            </el-form>
        </div>

        <el-dialog :visible.sync="passwordResetDialogVisible" width="30%">
            <div style="display: flex; align-items: center; justify-content: center;">
                <el-form ref="resetPasswordForm" :rules="resetPasswordRules" :model="resetPasswordModel" style="width: 80%;">
                    <el-form-item prop="mail">
                        <el-input prefix-icon="el-icon-s-promotion" v-model="resetPasswordModel.mail" style="font-size: 12px;"
                            placeholder="请输入邮箱"></el-input>
                    </el-form-item>
                
                    <el-form-item>
                        <el-button type="primary" style="width: 100%;" @click="resetPassword">重置</el-button>
                    </el-form-item>

                    <span>重置后的密码会发送至您的邮箱，请注意查收！</span>
                </el-form>
            </div>
        </el-dialog>
    </div>

</template>

<script>
export default {
    name: 'PasswordUpdate',
    components: {

    },
    data() {
        var validConfirmPasswordRule = (rule, value, callback) => {
            if (value === '') {
                callback(new Error('请确认密码！'));
            } else if (this.user.newPassword != value) {
                callback(new Error('密码不一致！'));
            } else {
                callback();
            }
        };

        return {
            user: {
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
                ],
            },

            resetPasswordRules: {
                mail: [
                    { required: true, message: '请输入邮箱', trigger: 'blur' }
                ]
            },

            resetPasswordModel: {
                mail: '',
            },

            passwordResetDialogVisible: false
        }

    },
    methods: {
        openResetDialog() {
            console.log('openResetDialog')
            this.passwordResetDialogVisible = true
        },

        updatePassword() {
            this.$refs['userRef'].validate((valid) => {
                if (valid) {
                    let token = localStorage.getItem("moyu_token");
                    let parseToken = JSON.parse(token)
                    this.request.post("user/updatePassword", {'id': parseToken.id, 'oldPassword': this.user.oldPassword, 'newPassword': this.user.newPassword}).then(response => {
                        if (response.code != '200') {
                            this.$message.error(response.message)
                        } else {
                            this.$message({
                                message: '修改密码成功！',
                                type: 'success'
                            });
                            window.location.replace("/");
                        }
                    })
                }
            })
        },

        resetPassword() {
            this.$refs['resetPasswordForm'].validate((valid) => {
                if (valid) {
                    let token = localStorage.getItem("moyu_token");
                    let parseToken = JSON.parse(token)
                    this.request.post("user/resetPassword", {'id': parseToken.id, 'mail': this.resetPasswordModel.mail}).then(response => {
                        if (response.code != '200') {
                            this.$message.error(response.message)
                        } else {
                            this.$message({
                                message: '重置密码成功！',
                                type: 'success'
                            });
                            localStorage.removeItem('moyu_token')
                            window.location.replace("/");
                        }
                    })
                }
            })
        }
    }
}
</script>

<style scoped>

</style>