<template>
    <div style="display: flex;">
        <!-- 爬取的热点信息 -->
        <div style="width: 55%; margin-left: 5%;">
            <div style="width: 100%; display: flex;">
                <el-card class="box-card" style="flex: 1;">
                    <div>
                        <div style="display: flex; align-items: center;">
                            <div style="flex: 1; display: flex;  justify-content: flex-start; padding-left: 5px;">
                                <img src="@/assets/zhihu_logo.jpg"
                                    style="width: 28px;height: 28px; border-radius: 5px;">
                                <span
                                    style="color: #0e78f2; margin-left: 8px; font-weight: 2000; margin-top: 2px; font-family: 'Avenir', Helvetica, Arial, sans-serif;">知乎热榜</span>
                            </div>

                            <div style="flex: 1; display: flex; justify-content: flex-end;">
                                <el-button icon="el-icon-refresh" size="mini" circle style="float: right inherit;"
                                    @click="zhihuList"></el-button>
                            </div>
                        </div>

                        <div style="margin-top: 10px; cursor: pointer;">
                            <el-table :data="zhihuHotData" style="width: 100%" @row-click="handleRowClick"
                                :show-header="false" height="400">
                                <el-table-column prop="sequ" label="序号" width="40">
                                </el-table-column>
                                <el-table-column prop="title" label="热点">
                                </el-table-column>
                                <el-table-column prop="showReadNum" label="热度" width="80">
                                </el-table-column>
                            </el-table>
                        </div>

                    </div>
                </el-card>
                <el-card class="box-card" style="width: 30%; margin-left: 50px; flex: 1;">
                    <div style="width: 100%;">
                        <div style="display: flex; align-items: center;">
                            <div style="flex: 1; display: flex;  justify-content: flex-start; padding-left: 5px;">
                                <img src="@/assets/toutiao_logo.jpg"
                                    style="width: 28px;height: 28px; border-radius: 5px;">
                                <span
                                    style="color: #fd373b; margin-left: 8px; font-weight: 2000; margin-top: 2px; font-family: 'Avenir', Helvetica, Arial, sans-serif;">头条热榜</span>
                            </div>

                            <div style="flex: 1; display: flex; justify-content: flex-end;">
                                <el-button icon="el-icon-refresh" size="mini" circle style="float: right inherit;"
                                    @click="touTiaoList"></el-button>
                            </div>
                        </div>

                        <div style="margin-top: 10px; cursor: pointer;">
                            <el-table :data="toutiaoHotData" style="width: 100%" @row-click="handleRowClick"
                                :show-header="false" height="400">
                                <el-table-column prop="sequ" label="序号" width="40">
                                </el-table-column>
                                <el-table-column prop="title" label="热点">
                                </el-table-column>
                                <el-table-column prop="showReadNum" label="热度" width="80">
                                </el-table-column>
                            </el-table>
                        </div>

                    </div>
                </el-card>
            </div>

            <!-- 第二行 -->
            <div style="width: 100%; display: flex; margin-top: 30px;">
                <el-card class="box-card" style="flex: 1;">
                    <div>
                        <div style="display: flex; align-items: center;">
                            <div style="flex: 1; display: flex;  justify-content: flex-start; padding-left: 5px;">
                                <img src="@/assets/weibo_logo.jpg"
                                    style="width: 28px;height: 28px; border-radius: 5px;">
                                <span
                                    style="color: #f8ce1e; margin-left: 8px; font-weight: 2000; margin-top: 2px;">微博热搜</span>
                            </div>

                            <div style="flex: 1; display: flex; justify-content: flex-end;">
                                <el-button icon="el-icon-refresh" size="mini" circle style="float: right inherit;"
                                    @click="weiBoList"></el-button>
                            </div>
                        </div>

                        <div style="margin-top: 10px; cursor: pointer;">
                            <el-table :data="weiboHotData" style="width: 100%;" @row-click="handleRowClick"
                                :show-header="false" height="400">
                                <el-table-column prop="sequ" label="序号" width="60">
                                </el-table-column>
                                <el-table-column prop="title" label="热点">
                                </el-table-column>
                                <el-table-column prop="showReadNum" label="热度" width="80">
                                </el-table-column>
                            </el-table>
                        </div>

                    </div>
                </el-card>

                <el-card class="box-card" style="width: 30%; margin-left: 50px; flex: 1;">
                    <div style="width: 100%;">
                        <div style="display: flex; align-items: center;">
                            <div style="flex: 1; display: flex;  justify-content: flex-start; padding-left: 5px;">
                                <img src="@/assets/hupu_logo.jpg" style="width: 28px;height: 28px; border-radius: 5px;">
                                <span
                                    style="color: #eb061a; margin-left: 8px; font-weight: 2000; margin-top: 2px; font-family: 'Avenir', Helvetica, Arial, sans-serif;">
                                    虎扑热帖
                                </span>
                            </div>

                            <div style="flex: 1; display: flex; justify-content: flex-end;">
                                <el-button icon="el-icon-refresh" size="mini" circle
                                    style="float: right inherit;" @click="huPuList"></el-button>
                            </div>
                        </div>

                        <div style="margin-top: 10px; cursor: pointer;">
                            <el-table :data="hupuHotData" style="width: 100%" @row-click="handleRowClick"
                                :show-header="false" height="400">
                                <el-table-column prop="sequ" label="序号" width="40">
                                </el-table-column>
                                <el-table-column prop="title" label="热点">
                                </el-table-column>
                                <el-table-column prop="showReadNum" label="热度" width="80">
                                </el-table-column>
                            </el-table>
                        </div>

                    </div>
                </el-card>
            </div>


            <!-- 第三行 -->
            <div style="width: 100%; display: flex; margin-top: 30px;">
                <el-card class="box-card" style="flex: 1;">
                    <div>
                        <div style="display: flex; align-items: center;">
                            <div style="flex: 1; display: flex;  justify-content: flex-start; padding-left: 5px;">
                                <img src="@/assets/caijing_icon.jpg"
                                    style="width: 28px;height: 28px; border-radius: 5px;">
                                <span
                                    style="color: #136dde; margin-left: 8px; font-weight: 2000; margin-top: 2px;">财经优选</span>
                            </div>

                            <div style="flex: 1; display: flex; justify-content: flex-end;">
                                <el-button icon="el-icon-refresh" size="mini" circle style="float: right inherit;"
                                    @click="caijingList"></el-button>
                            </div>
                        </div>

                        <div style="margin-top: 10px; cursor: pointer;">
                            <el-table :data="caijingData" style="width: 100%;" @row-click="handleRowClick"
                                :show-header="false" height="400">
                                <el-table-column prop="sequ" label="序号" width="60">
                                </el-table-column>
                                <el-table-column prop="title" label="热点">
                                </el-table-column>
                                <el-table-column prop="showReadNum" label="热度" width="80">
                                </el-table-column>
                            </el-table>
                        </div>

                    </div>
                </el-card>

                <el-card class="box-card" style="width: 30%; margin-left: 50px; flex: 1;">
                    <div style="width: 100%;">
                        <div style="display: flex; align-items: center;">
                            <div style="flex: 1; display: flex;  justify-content: flex-start; padding-left: 5px;">
                                <img src="@/assets/junshi_icon.jpg" style="width: 28px;height: 28px; border-radius: 5px;">
                                <span
                                    style="color: #e84a42; margin-left: 8px; font-weight: 2000; margin-top: 2px; font-family: 'Avenir', Helvetica, Arial, sans-serif;">
                                    军情速递
                                </span>
                            </div>

                            <div style="flex: 1; display: flex; justify-content: flex-end;">
                                <el-button icon="el-icon-refresh" size="mini" circle
                                    style="float: right inherit;" @click="junshiList"></el-button>
                            </div>
                        </div>

                        <div style="margin-top: 10px; cursor: pointer;">
                            <el-table :data="junshiData" style="width: 100%" @row-click="handleRowClick"
                                :show-header="false" height="400">
                                <el-table-column prop="sequ" label="序号" width="40">
                                </el-table-column>
                                <el-table-column prop="title" label="热点">
                                </el-table-column>
                                <el-table-column prop="showReadNum" label="热度" width="80">
                                </el-table-column>
                            </el-table>
                        </div>

                    </div>
                </el-card>
            </div>


        </div>

        <!-- 今日时间，天气，在线人数等信息 -->
        <div style="width: 30%; margin-left: 15%;">
            <el-card class="box-card" shadow="always">
                <div slot="header" class="clearfix">
                    <span style="font-size: 20px; color: #c5967c;">实时在线统计</span>
                    <el-switch style="float: right; margin-top: 2px; padding: 3px 0" v-model="isonlineDisplay"
                        active-color="#13ce66" inactive-color="#b2adaa" @change="changeOnlineDisplay"></el-switch>
                </div>

                <div id="onlineDisplay" style="margin-top: 5px;">
                    <p style="font-size: 17px; color: #bd9b77;">当前时间：{{ nowTime }}</p>

                    <p style="font-size: 17px; color: #bd9b77; cursor: pointer;" @click="showLocations">实时在线人数：{{ onlineCount }} </p>

                    <p style="font-size: 14px; color: green;">tip: 上面在线人数点击会展示在线用户所在的地区分布哦</p>
                </div>
            </el-card>

            <el-card class="box-card" shadow="always" style="text-align: center;">
                <div slot="header" class="clearfix">
                    <span style="font-size: 20px; color:  #c5967c;">老黄历</span>
                    <el-switch style="float: right; margin-top: 2px; padding: 3px 0" v-model="isOpenLaoHuangLi"
                        active-color="#13ce66" inactive-color="#b2adaa"
                        @change="changeLaoHuangLiShowStataus"></el-switch>
                </div>
                <div id="huangLi">
                    <p style="font-weight: 2000;">{{ gongli }}</p>
                    <p style="font-weight: 2000;">{{ nongli }}</p>
                    <div style="display: flex; margin-top: 3px;" @click="viewHuangLi">
                        <div style="flex: 1; color: green;">
                            <el-row style="font-size: 20px; margin-bottom: 5px;">
                                宜
                            </el-row>

                            <div class="yiclass">
                                <el-row v-for="(item, index) in yiData" :key="index" class="item-row">
                                    {{ item }}
                                </el-row>
                            </div>
                        </div>
                        <div style="flex: 1; color: #e17a46;">
                            <el-row style="font-size: 20px; margin-bottom: 5px;">
                                忌
                            </el-row>

                            <div class="yiclass">
                                <el-row v-for="(item, index) in jiData" :key="index" class="item-row">
                                    {{ item }}
                                </el-row>
                            </div>
                        </div>
                    </div>
                </div>
            </el-card>

            <el-card class="box-card" shadow="always">
                <div slot="header" class="clearfix">
                    <span style="font-size: 20px; color: #c5967c;">假期提醒</span>
                    <el-switch style="float: right; margin-top: 2px; padding: 3px 0" v-model="isHolidayDisplay"
                        active-color="#13ce66" inactive-color="#b2adaa" @change="changeHolidayDisplay"></el-switch>
                </div>

                <div id="holidayDisplay" style="margin-top: 5px;">
                    <el-row
                        style="font-size: 15px; margin-bottom: 10px; color: #bd9b77">不是所有的坚持都需要理由，但每一次的假期都值得我们满怀期待。打工人，是时候放下工作的重担，让心灵去旅行，享受那份久违的宁静与自由。</el-row>

                    <div style="cursor: pointer;" @click="viewHoliday">
                        <el-row style="font-size: 18px;" v-for="(item, index) in holidays" :key="index"
                            class="item-row">
                            {{ item }}
                        </el-row>
                    </div>

                    <p style="font-size: 14px; color: green;">tip：点击上面的假期时间可以跳转日历哦</p>
                </div>
            </el-card>

            <el-card class="box-card" shadow="always">
                <div slot="header" class="clearfix">
                    <span style="font-size: 20px; color: #c5967c;">关于我们</span>
                    <el-switch style="float: right; margin-top: 2px; padding: 3px 0" v-model="isAboutUsDisplay"
                        active-color="#13ce66" inactive-color="#b2adaa" @change="changeAboutUsDisplay"></el-switch>
                </div>

                <div id="aboutUsDisplay" style="margin-top: 5px; font-size: 18px; ">
                    本网站由资深摸鱼人搭建，还在逐步完善中，有任何想法可以通过吐槽留言给我们，您的宝贵意见是我们前进的动力！
                    <p>联系邮箱：1912758962@qq.com</p>
                    <span style="color: #0f9876; cursor: pointer;" @click="supportMoney">
                        我要赞助
                    </span>
                </div>
            </el-card>
        </div>

        <el-dialog title="感谢您的赞助，让我们变得更好" :visible.sync="dialogVisible" width="50%">
            <div style="display: flex;">
                <div style="flex: 1;">
                    <!-- <p style="font-weight: 3000;">微信</p> -->
                    <img src="@/assets/wechat_payment_code.jpg" style="width: 400px;height: 400px;">
                </div>
                <div style="flex: 1; margin-left: 20px;">
                    <!-- <p style="font-weight: 3000;">支付宝</p> -->
                    <img src="@/assets/zhifuao_payment_code.jpg" style="width: 400px;height: 500px;">
                </div>
            </div>
        </el-dialog>

        <!-- 在线人数地区分布统计图 -->
        <el-dialog title="在线人数地区分布统计" :visible.sync="locationDialogVisible" width="50%">
            <!--国内分布-->
            <div>
                <canvas id="chinaLocations"></canvas>
            </div>

            <!--国外分布， 基本为网络攻击-->
            <div style="margin-top: 20px;">
                <canvas id="otherCountryLocations"></canvas>
            </div>

        </el-dialog>

    </div>
</template>

<script>

import Chart from 'chart.js';

export default {
    data() {
        return {
            zhihuHotData: [{
                sequ: 1,
                title: '习近平主持召开中共中央政治局会议',
                showReadNum: '142 万热度',
                url: 'https://www.toutiao.com/article/7406327891955089972'
            }, {
                sequ: 2,
                title: '柬埔寨运河项目为何又火了？',
                showReadNum: '137 万热度',
                url: 'https://www.toutiao.com/trending/7403646040848613426/'
            }, {
                sequ: 3,
                title: '沙利文下周将访华',
                showReadNum: '126 万热度',
                url: 'https://www.toutiao.com/trending/7406494456549035530/?rank=2&log_from=4866fe2c718ba_1724470197820'
            }, {
                sequ: 4,
                title: '我国跨境电商这样跑出“加速度”',
                showReadNum: '115 万热度',
                url: 'https://www.toutiao.com/article/7406261818844250678'
            },
            {
                sequ: 5,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 6,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 7,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 8,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 9,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 10,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 11,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 12,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            }
            ],
            toutiaoHotData: [{
                sequ: 1,
                title: '习近平主持召开中共中央政治局会议',
                showReadNum: '142 万热度',
                url: 'https://www.toutiao.com/article/7406327891955089972'
            }, {
                sequ: 2,
                title: '柬埔寨运河项目为何又火了？',
                showReadNum: '137 万热度',
                url: 'https://www.toutiao.com/trending/7403646040848613426/'
            }, {
                sequ: 3,
                title: '沙利文下周将访华',
                showReadNum: '126 万热度',
                url: 'https://www.toutiao.com/trending/7406494456549035530/?rank=2&log_from=4866fe2c718ba_1724470197820'
            }, {
                sequ: 4,
                title: '我国跨境电商这样跑出“加速度”',
                showReadNum: '115 万热度',
                url: 'https://www.toutiao.com/article/7406261818844250678'
            },
            {
                sequ: 5,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 6,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 7,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 8,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 9,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 10,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 11,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 12,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            }
            ],
            weiboHotData: [{
                sequ: 1,
                title: '习近平主持召开中共中央政治局会议',
                showReadNum: '142 万热度',
                url: 'https://www.toutiao.com/article/7406327891955089972'
            }, {
                sequ: 2,
                title: '柬埔寨运河项目为何又火了？',
                showReadNum: '137 万热度',
                url: 'https://www.toutiao.com/trending/7403646040848613426/'
            }, {
                sequ: 3,
                title: '沙利文下周将访华',
                showReadNum: '126 万热度',
                url: 'https://www.toutiao.com/trending/7406494456549035530/?rank=2&log_from=4866fe2c718ba_1724470197820'
            }, {
                sequ: 4,
                title: '我国跨境电商这样跑出“加速度”',
                showReadNum: '115 万热度',
                url: 'https://www.toutiao.com/article/7406261818844250678'
            },
            {
                sequ: 5,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 6,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 7,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 8,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 9,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 10,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 11,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 12,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            }
            ],
            hupuHotData: [{
                sequ: 1,
                title: '习近平主持召开中共中央政治局会议',
                showReadNum: '142 万热度',
                url: 'https://www.toutiao.com/article/7406327891955089972'
            }, {
                sequ: 2,
                title: '柬埔寨运河项目为何又火了？',
                showReadNum: '137 万热度',
                url: 'https://www.toutiao.com/trending/7403646040848613426/'
            }, {
                sequ: 3,
                title: '沙利文下周将访华',
                showReadNum: '126 万热度',
                url: 'https://www.toutiao.com/trending/7406494456549035530/?rank=2&log_from=4866fe2c718ba_1724470197820'
            }, {
                sequ: 4,
                title: '我国跨境电商这样跑出“加速度”',
                showReadNum: '115 万热度',
                url: 'https://www.toutiao.com/article/7406261818844250678'
            },
            {
                sequ: 5,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 6,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 7,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 8,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 9,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 10,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 11,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 12,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            }
            ],
            junshiData: [{
                sequ: 1,
                title: '习近平主持召开中共中央政治局会议',
                showReadNum: '142 万热度',
                url: 'https://www.toutiao.com/article/7406327891955089972'
            }, {
                sequ: 2,
                title: '柬埔寨运河项目为何又火了？',
                showReadNum: '137 万热度',
                url: 'https://www.toutiao.com/trending/7403646040848613426/'
            }, {
                sequ: 3,
                title: '沙利文下周将访华',
                showReadNum: '126 万热度',
                url: 'https://www.toutiao.com/trending/7406494456549035530/?rank=2&log_from=4866fe2c718ba_1724470197820'
            }, {
                sequ: 4,
                title: '我国跨境电商这样跑出“加速度”',
                showReadNum: '115 万热度',
                url: 'https://www.toutiao.com/article/7406261818844250678'
            },
            {
                sequ: 5,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 6,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 7,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 8,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 9,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 10,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 11,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 12,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            }
            ],
            caijingData: [{
                sequ: 1,
                title: '习近平主持召开中共中央政治局会议',
                showReadNum: '142 万热度',
                url: 'https://www.toutiao.com/article/7406327891955089972'
            }, {
                sequ: 2,
                title: '柬埔寨运河项目为何又火了？',
                showReadNum: '137 万热度',
                url: 'https://www.toutiao.com/trending/7403646040848613426/'
            }, {
                sequ: 3,
                title: '沙利文下周将访华',
                showReadNum: '126 万热度',
                url: 'https://www.toutiao.com/trending/7406494456549035530/?rank=2&log_from=4866fe2c718ba_1724470197820'
            }, {
                sequ: 4,
                title: '我国跨境电商这样跑出“加速度”',
                showReadNum: '115 万热度',
                url: 'https://www.toutiao.com/article/7406261818844250678'
            },
            {
                sequ: 5,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 6,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 7,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 8,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 9,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 10,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            },
            {
                sequ: 11,
                title: '韩国发生车祸5死9伤 死者中3人中国籍',
                showReadNum: '113 万热度',
                url: 'https://www.toutiao.com/trending/7406564316230454821/?rank=5&log_from=d85e772d4f414_1724480110173'
            },
            {
                sequ: 12,
                title: '救人牺牲英雄栾留伟妻子拒收捐款',
                showReadNum: '112 万热度',
                url: 'https://www.toutiao.com/trending/7406161809487003667/?rank=1&log_from=1fca67f8d0271_1724480169460'
            }
            ],

            gongli: '',
            nongli: '',
            yiData: [],
            jiData: [],

            isOpenLaoHuangLi: true,
            isHolidayDisplay: true,
            isAboutUsDisplay: true,
            isonlineDisplay: true,

            holidays: [],
            dialogVisible: false,
            locationDialogVisible: false,
            nowTime: new Date().toLocaleTimeString(),
            onlineCount: '',
            chinaLabel: [],
            chinaLabelData: [],
            otherCountryLabel: [],
            otherCountryLabelData: []

        }
    },
    methods: {
        handleRowClick(row, event, column) {
            window.open(row.url, '_blank');
        },

        changeLaoHuangLiShowStataus(value) {
            if (value) {
                document.getElementById("huangLi").style.display = 'block'
            } else {
                document.getElementById("huangLi").style.display = 'none'
            }
        },

        changeHolidayDisplay(value) {
            if (value) {
                document.getElementById("holidayDisplay").style.display = 'block'
            } else {
                document.getElementById("holidayDisplay").style.display = 'none'
            }
        },

        changeAboutUsDisplay(value) {
            if (value) {
                document.getElementById("aboutUsDisplay").style.display = 'block'
            } else {
                document.getElementById("aboutUsDisplay").style.display = 'none'
            }
        },

        changeOnlineDisplay(value) {
            if (value) {
                document.getElementById("onlineDisplay").style.display = 'block'
            } else {
                document.getElementById("onlineDisplay").style.display = 'none'
            }
        },

        //查看黄历
        viewHuangLi() {
            window.open(row.url, '_blank');
        },

        viewHoliday() {
            window.open('https://www.baidu.com/s?ie=UTF-8&wd=%E6%97%A5%E5%8E%86', '_blank');
        },

        supportMoney() {
            this.dialogVisible = true;
        },

        //查询请求
        zhihuList() {
            this.request.get("zhihu/list").then(response => {           
                if (response.code != '200') {
                    this.$message.error("查询知乎热点失败！")
                } else {
                    this.zhihuHotData = response.data
                }
            })
        },

        touTiaoList() {
            this.request.get("toutiao/list").then(response => {
                if (response.code != '200') {
                    this.$message.error("查询头条热点失败！")
                } else {
                    this.toutiaoHotData = response.data
                }
            })
        },

        weiBoList() {
            this.request.get("weibo/list").then(response => {
                if (response.code != '200') {
                    this.$message.error("查询微博热点失败！")
                } else {
                    this.weiboHotData = response.data
                }
            })
        },

        huPuList() {
            this.request.get("hupu/list").then(response => {
                if (response.code != '200') {
                    this.$message.error("查询虎扑热帖失败！")
                } else {
                    this.hupuHotData = response.data
                }
            })
        },

        junshiList() {
            this.request.get("junshi/list").then(response => {
                if (response.code != '200') {
                    this.$message.error("查询军事新闻失败！")
                } else {
                    this.junshiData = response.data;
                }
            })
        },

        caijingList() {
            this.request.get("caijing/list").then(response => {
                if (response.code != '200') {
                    this.$message.error("查询财经新闻失败！")
                } else {
                    this.caijingData = response.data;
                }
            })
        },

        laoHuangLiData() {
            this.request.get("laoHuangLi/query").then(response => {
                if (response.code != '200') {
                    this.$message.error("查询老黄历失败！")
                } else {
                    this.gongli = response.data.gongli;
                    this.nongli = response.data.nongli;
                    this.yiData = response.data.yishiList;
                    this.jiData = response.data.jishiList;
                }
            })
        },

        holidayData() {
            this.request.get("holiday/query").then(response => {
                if (response.code != '200') {
                    this.$message.error("查询节假日失败！")
                } else {
                    this.holidays = response.data;
                }
            })
        },

        countOnlinePerson() {
            this.request.get("countOnlinePerson/count").then(response => {
                if (response.code != '200') {
                    this.$message.error("查询实时在线人数失败！")
                } else {
                    this.onlineCount = response.data;
                }
            })
        },

        showLocations() {
            this.locationDialogVisible = true;

            this.request.get("ipAnalyze/statistics").then(response => {
                if (response.code != '200') {
                    this.$message.error("查询在线人数分布数据失败！");
                    this.locationDialogVisible = false;
                } else {
                    let tempData = response.data;

                    let chinaData = tempData.china
                    let otherCountryData = tempData.otherCountry

                    let chinaLabelTemp = [];
                    let chinaLabelDataTemp = [];
                    for (var key in chinaData) {
                        var temp = chinaData[key]
                        chinaLabelTemp.push(temp.locationName);
                        chinaLabelDataTemp.push(temp.total)
                    }

                    let otherCountryLabelTemp = [];
                    let otherCountryLabelDataTemp = [];
                    for (var key in otherCountryData) {
                        var temp = otherCountryData[key]
                        otherCountryLabelTemp.push(temp.locationName);
                        otherCountryLabelDataTemp.push(temp.total)
                    }

                    this.chinaLabel = chinaLabelTemp;
                    this.chinaLabelData = chinaLabelDataTemp;
                    this.otherCountryLabel = otherCountryLabelTemp;
                    this.otherCountryLabelData = otherCountryLabelDataTemp;

                    var chinaLocations = document.getElementById("chinaLocations");
                    new Chart(chinaLocations, {
                        type: 'bar', //柱状图
                        data: {
                            labels: this.chinaLabel,
                            datasets: [{
                                label: '国内在线用户分布',
                                data: this.chinaLabelData,
                                // backgroundColor: [
                                //     'rgba(255, 99, 132, 0.2)',
                                //     'rgba(54, 162, 235, 0.2)',
                                //     'rgba(255, 206, 86, 0.2)',
                                //     'rgba(75, 192, 192, 0.2)',
                                //     'rgba(153, 102, 255, 0.2)',
                                //     'rgba(255, 159, 64, 0.2)'
                                // ],
                                // borderColor: [
                                //     'rgba(255,99,132,1)',
                                //     'rgba(54, 162, 235, 1)',
                                //     'rgba(255, 206, 86, 1)',
                                //     'rgba(75, 192, 192, 1)',
                                //     'rgba(153, 102, 255, 1)',
                                //     'rgba(255, 159, 64, 1)'
                                // ],
                                barThickness: 30,
                                categoryPercentage: 0.5,
                                barPercentage: 0.5,
                                hoverBackgroundColor: 'rgba(255,255,0,0.5)'
                            }]
                        },
                        options: {
                            scales: {
                                yAxes: [{
                                    ticks: {
                                        beginAtZero: true
                                    }
                                }]
                            }
                        }
                    });


                    var otherCountryLocations = document.getElementById("otherCountryLocations");
                    new Chart(otherCountryLocations, {
                        type: 'bar', //柱状图
                        data: {
                            labels: this.otherCountryLabel,
                            datasets: [{
                                label: '国外在线用户分布（基本为网络攻击）',
                                data: this.otherCountryLabelData,
                                // backgroundColor: [
                                //     'rgba(255, 99, 132, 0.2)',
                                //     'rgba(54, 162, 235, 0.2)',
                                //     'rgba(255, 206, 86, 0.2)',
                                //     'rgba(75, 192, 192, 0.2)',
                                //     'rgba(153, 102, 255, 0.2)',
                                //     'rgba(255, 159, 64, 0.2)'
                                // ],
                                // borderColor: [
                                //     'rgba(255,99,132,1)',
                                //     'rgba(54, 162, 235, 1)',
                                //     'rgba(255, 206, 86, 1)',
                                //     'rgba(75, 192, 192, 1)',
                                //     'rgba(153, 102, 255, 1)',
                                //     'rgba(255, 159, 64, 1)'
                                // ],
                                barThickness: 30,
                                categoryPercentage: 0.5,
                                barPercentage: 0.5,
                                hoverBackgroundColor: 'rgba(255,255,0,0.5)'
                            }]
                        },
                        options: {
                            scales: {
                                yAxes: [{
                                    ticks: {
                                        beginAtZero: true
                                    }
                                }]
                            }
                        },
                    });
                }
            });


        }


    },

    //页面加载好后触发的操作
    mounted() {
        this.zhihuList();
        this.touTiaoList();
        this.weiBoList();
        this.huPuList();
        this.junshiList();
        this.caijingList();
        this.laoHuangLiData();
        this.holidayData();
        this.countOnlinePerson();
    },

    //页面创建时
    created() {
        setInterval(() => {
            this.nowTime = new Date().toLocaleTimeString();
        }, 1000);

        //5分钟统计一次在线人数
        setInterval(() => {
            this.countOnlinePerson();
        }, 1000 * 60 * 3);
    }
}
</script>


<!-- scoped 可以解决样式残留问题，样式只在当前页面生效 -->
<style scoped>
.box-card {
    width: 100%;
    margin-bottom: 20px;
}

.yiclass {
    height: 150px;
    overflow-y: auto;
    scrollbar-gutter: stable;
}

::-webkit-scrollbar {
    /* 隐藏滚动条 */
    display: none;
}
</style>