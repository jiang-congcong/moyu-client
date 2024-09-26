<template>
    <div>
        <div style="float: right; margin-bottom: 10px;">
            操作按钮区域
        </div>
        <el-table highlight-current-row :data="users" style="width: 100%; font-size: 15px; ">
            <el-table-column align="center" prop="id" label="id" >
            </el-table-column>
            <el-table-column align="center" prop="name" label="用户名" >
            </el-table-column>
            <el-table-column align="center" prop="birthday" label="生日" >
            </el-table-column>
            <el-table-column align="center" prop="phone" label="手机号" >
            </el-table-column>
            <el-table-column align="center" prop="address" label="地址" >
            </el-table-column>
            <el-table-column align="center" prop="mail" label="邮箱">
            </el-table-column>
            <el-table-column align="center" fixed="right" label="操作">
                <template slot-scope="scope">
                    <el-button @click="handleClick(scope.row)" type="text" size="medium">查看</el-button>
                    <el-button type="text" size="medium" @click="edit(scope.row)">编辑</el-button>
                    <el-button type="text" size="medium" @click="delete (scope.row)">删除</el-button>
                </template>
            </el-table-column>
        </el-table>

        <el-pagination style="float: right; margin-top: 10px;" @current-change="switchPage" :current-page=currentPage :page-size=pageSize :pager-count="6" layout="prev, pager, next" :total=total>
        </el-pagination>
    </div>
</template>

<script>
export default {
    data() {
        return {
            users: [{
                id: 1,
                birthday: '2016-05-02',
                name: '王小虎',
                phone: '13627456398',
                address: '上海市普陀区金沙江路 1518 弄',
                mail: 'qq@qq.com'
            }, {
                id: 2,
                birthday: '2016-05-03',
                name: '王小虎1',
                phone: '13627456399',
                address: '上海市普陀区金沙江路 1518 弄',
                mail: 'qq1@qq.com'
            }, {
                id: 3,
                birthday: '2016-05-04',
                name: '王小虎2',
                phone: '13627456399',
                address: '上海市普陀区金沙江路 1518 弄',
                mail: 'qq3@qq.com'
            }],
            total : 200,
            pageSize: 20,
            currentPage: 1
        }
    },
    methods: {
        switchPage(switchPageNum) {
            this.currentPage = switchPageNum
            console.log(switchPageNum)
        }
    }
}
</script>

<style></style>
