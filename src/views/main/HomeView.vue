<template>
    <div>
        <div style="box-shadow:0 0 10px rgba(0,0,0,1); padding: 10px 20px; border-radius: 5px; margin-bottom: 10px;">
            哈喽，快乐的打工人！
        </div>
        <div style="display: flex;">
            <el-card style="width: 50%; margin-right: 10px;">
                <div>
                    <span>模块一</span>
                </div>
            </el-card>

            <el-card style="width: 50%;">
                <div>
                    <span>模块二</span>
                </div>
                <div>
                    <el-table :data="users" stripe style="width: 100%">
                        <el-table-column prop="id" label="用户ID" width="180"></el-table-column>
                        <el-table-column prop="username" label="姓名" width="180"></el-table-column>
                        <el-table-column prop="phone" label="手机号" width="180"></el-table-column>
                        <el-table-column prop="address" label="地址"></el-table-column>
                    </el-table>
                </div>
            </el-card>
        </div>
    </div>
</template>


<script>

export default {
    data() {
        return {
            isCollapse: false,
            asideWidth: '200px',
            collapseIcon: 'el-icon-s-fold',
            users: [
                {
                    id: '001',
                    username: '王小虎',
                    address: '上海市普陀区金沙江路 1518 弄',
                    phone: '13728398652'
                }
            ]
        }
    },
    //页面加载好后触发的操作
    mounted() {
        this.request.get('person/list').then(response => {
            let res = response;
            console.log(res)
            if (res.code !== 200) {
                console.log("请求错误！")
            } else {
                if (typeof res === 'string') {
                    res = res ? JSON.parse(res) : res
                }

                this.users = res.data;
            }
        }).catch(error => {
            // 请求失败时处理错误  
            console.error('There was an error!', error);
        });
    },
    methods: {
    
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

.el-dropdown-menu__item,
.el-menu-item {
    padding: 0 40px 0 0 !important;
}

.el-menu--inline .el-menu-item {
    padding-left: 75px !important;
    text-align: left;
}
</style>