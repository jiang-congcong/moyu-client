<template>
    <div style=" display: flex; justify-content: center;">
        <el-card class="box-card" style="width: 40%; ">
            <div style="display: flex; justify-content: center; align-items: center; margin-bottom: 20px; font-size: 20px;font-weight: bold;"><span>个人信息</span></div>
            <el-form ref="personValidRef" :rules="rules" :model="person" label-width="80px" style="margin-right: 20px;">
                <el-form-item label="id" prop="id">
                    <el-input v-model="person.id" disabled></el-input>
                </el-form-item>
                <el-form-item label="用户名" prop="username">
                    <el-input v-model="person.username" disabled></el-input>
                </el-form-item>
                <el-form-item label="手机号" prop="phone">
                    <el-input v-model="person.phone"></el-input>
                </el-form-item>
                <el-form-item label="生日" prop="birthday">
                    <el-input v-model="person.birthday"></el-input>
                </el-form-item>
                <el-form-item label="地址" prop="address">
                    <el-input v-model="person.address"></el-input>
                </el-form-item>
                <el-form-item label="邮箱" prop="mail">
                    <el-input v-model="person.mail"></el-input>
                </el-form-item>
                <div style="display: flex; justify-content: center; align-items: center;">
                    <el-button type="primary" @click="confirm">确定</el-button>                                      
                    <el-button type="info" @click="cancel">取消</el-button>
                </div>
            </el-form>
        </el-card>
    </div>
</template>

<script>
export default {
    data() {
        return {
            person: {
                id: '1',
                birthday: '2016-05-02',
                username: '王小虎',
                phone: '13627456398',
                address: '上海市普陀区金沙江路 1518 弄',
                mail: 'qq@qq.com'
            },

            rules: {
                    username: [
                        { required: true, message: '请输入用户名', trigger: 'blur' }
                    ],
                    phone: [
                        { required: true, message: '请输入手机号', trigger: 'blur' },
                        { min: 11, max: 11, message: '手机号长度11位', trigger: 'blur' }
                    ],
                    address: [
                        { required: true, message: '请输入地址', trigger: 'blur' },
                    ],
                    mail: [
                        { required: true, message: '请输入邮箱', trigger: 'blur' },
                    ]
                }
        }
    },
    //页面加载好后触发的操作
    mounted() {

    },

    methods: {
        confirm() {
            this.$refs['personValidRef'].validate((valid) => {
                    if (valid) {
                        this.request.post("user/update", this.person).then(response => {
                            console.log(response)
                            if (response.code != '200') {
                                this.$message.error("修改用户信息失败！")
                            } else {
                                localStorage.setItem("token", JSON.stringify(response.data.token))
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