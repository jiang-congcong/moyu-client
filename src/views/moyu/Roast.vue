<template>
    <div style="display: flex; justify-content: center;">
        <!-- <el-card class="box-card">
            <div slot="header" class="clearfix">
                <span style="font-size: 20px; color: #c5967c;">我要吐槽</span>
            </div>

            <div style="margin-top: 5px; font-size: 18px; ">
                本网站由资深摸鱼人搭建，还在逐步完善中，有任何想法暂时可以通过下方邮箱发给我们，您的宝贵意见是我们前进的动力！
                <p>联系邮箱：1912758962@qq.com</p>     
            </div>
        </el-card> -->
    </div>

</template>
<script>
export default {
    data() {
        return {
            dialogVisible: false
        }
    },
    methods: {
    },

    //页面加载好后触发的操作
    mounted() {
    
    },
    created() {
        window.open('https://support.qq.com/product/667945', '_blank')
        this.$router.push("/");
    }
}
</script>

<!-- scoped 可以解决样式残留问题，样式只在当前页面生效 -->
<style scoped>
.box-card {
    width: 30%;
  }
</style>