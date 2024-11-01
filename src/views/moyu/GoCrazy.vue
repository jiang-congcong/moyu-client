<template>
  <div class="crazy-list">
    <el-alert title="本功能暂时只支持匿名，因此很多地方为无状态数据，有任何想法可以联系我们" type="warning" center show-icon
      style="margin-bottom: 10px;"></el-alert>

    <div style="display: flex; justify-content: center; align-items: center; height: 50px;">
      <el-input placeholder="请输入关键词" prefix-icon="el-icon-search" v-model="keyword" @keyup.enter.native="search"
        style="width: 40%; margin-left: 10px;"></el-input>
      <el-button type="primary" icon="el-icon-search" style="margin-left: 20px;" @click="search">搜索</el-button>
      <el-button type="primary" icon="el-icon-edit-outline" style="margin-left: 20px;"
        @click="goCrazyDialog = true">我要发疯</el-button>
      <el-button type="primary" icon="el-icon-view" style="margin-left: 20px;" @click="myComment">我的发帖</el-button>
    </div>

    <div style="margin-top: 10px;  font-size: 15px; ">
      <el-row v-for="item in goCrazys" :key="item.id"
        style="width: 100%; margin-bottom: 5px; display: flex; justify-content: center;">
        <el-card style="width: 80%; height: 10%;" shadow="always">
          <div>
            <div style="height: 80%; border-bottom: 1px solid #c2c8d1; ">
              <!--用户头像和用户名 时间等-->
              <div style="height: 30%;  display: flex;">
                <!--头像-->
                <div style="width: 6%; ">
                  <img :src="item.headImageUrl" style="width: 50px; height: 50px; border-radius: 30px; ">
                </div>
                <div style="width: 80%; margin-left: 3px; font-weight: 500; font-size: 16px;">
                  <div style="text-align: left; margin-top: 3px;">{{ item.userName }}</div>
                  <div style="text-align: left; margin-top: 5px; color: #909090;"><span style="font-size: 14px;">{{
                      item.betweenTime }}</span><span style="font-size: 12px; margin-left: 1px;">{{ item.betweenTimeUnit
                      }}</span></div>
                </div>
              </div>

              <!-- 内容 -->
              <div
                style="height: 70%;  margin-left: 6%; margin-top: 12px; margin-bottom: 22px; text-align: left; font-size: 16px; font-weight: 500; cursor: pointer;" @click="viewDetail(item)">
                {{ item.content }}
              </div>
            </div>

            <!-- 底部区域 点赞 评论啥的 -->
            <div style="height: 10%; margin-top: 10px; display: flex;">
              <div style="flex: 1; width: 20%;">
                <el-button :class="item.starIcon" @click="handleStar(item)" style="border: none;" size="medium">
                  {{ item.starNum }}
                </el-button>
              </div>
              <div style="flex: 1; width: 20%;">
                <el-button icon="el-icon-s-comment" @click="comment(item)" style="border: none;" size="medium">
                  {{ item.commentNum }}
                </el-button>
              </div>
              <div style="flex: 1; width: 20%;">
                <el-button icon="el-icon-view" type="text"
                  style="border: none; cursor: default; color: black; margin-left: 20px;" size="medium">
                  {{ item.readNum }}
                </el-button>
              </div>
              <div style="flex: 1; width: 20%;">
                <el-button icon="el-icon-time" type="text"
                  style="border: none; cursor: default; color: black; margin-left: 30px;" size="medium">
                  {{ item.createTime }}
                </el-button>
              </div>
            </div>
          </div>
        </el-card>
      </el-row>

      <!-- 分页 -->
      <el-pagination :page-size="20" :pager-count="7" layout="prev, pager, next" :total="total"
        :current-page.sync="currentPage" @current-change="handleCurrentChange"
        style="border: solidv 2px black; width: 90%; margin-top: 30px; display: flex; justify-content: flex-end;"></el-pagination>
    </div>

    <!--详情抽屉-->
    <el-drawer title="详情" :visible.sync="detailDrawer" :direction="direction">
      <div>
        <div style="border-bottom: 2px dashed #ccc; margin-bottom: 20px;">
          <p style="text-align: left; margin-left: 10px;">{{ detailItem.content }}</p>
        </div>
        <el-input type="textarea" :autosize="{ minRows: 4, maxRows: 10 }" placeholder="欢迎评论吐槽，共诉心声"
          v-model="inputComment"></el-input>
        <div
          style="display: flex; justify-content: flex-end; height: 30px; width: 100%; font-size: 10px; margin-top: 2px; margin-bottom: 10px;">
          <el-button type="danger" size="mini" round icon="el-icon-edit" @click="crazyComment">
            发表评论
          </el-button>
        </div>

      </div>
      <p style="text-align: left; margin-left: 10px;">评论历史</p>
      <el-row v-for="item in commentData" :key="item.id" style="width: 100%;">
        <el-card style=" height: 3%; margin-top: 6px; font-size: 12px;" shadow="always">
          <div>
            <!--用户头像和用户名 时间等-->
            <div style="height: 30%;  display: flex;">
                <!--头像-->
                <div style="width: 10%; ">
                  <img :src="item.headImageUrl" style="width: 40px; height: 40px; border-radius: 30px; ">
                </div>
                <div style="width: 80%; margin-left: 3px; font-weight: 500; font-size: 12px;">
                  <div style="text-align: left; margin-top: 3px;">{{ item.userName }}</div>
                  <div style="text-align: left; margin-top: 5px; color: #909090;"><span style="font-size: 12px;">{{
                      item.betweenTime }}</span><span style="font-size: 12px; margin-left: 1px;">{{ item.betweenTimeUnit
                      }}</span></div>
                </div>
              </div>

            <!--评论内容-->
            <div style="height: 80%; margin-left: 10%;">
              <p style="display: flex; justify-content: flex-start; align-items: center; font-size: 14px; font-weight: 400;">{{ item.comment }}</p>
              <div style="display: flex; justify-content: flex-end; margin-right: 15px;">
                <el-button icon="el-icon-time" type="text" style="border: none; cursor: default; color: black; font-size: 12px; font-weight: 400;"
                size="medium">
                  {{ item.createTime }}
                </el-button>
              </div>
            </div>

          </div>
        </el-card>

      </el-row>
    </el-drawer>

    <!--评论抽屉-->
    <el-drawer title="评论区" :visible.sync="drawer" :direction="direction">
      <div>
        <el-input type="textarea" :autosize="{ minRows: 4, maxRows: 10 }" placeholder="欢迎评论吐槽，共诉心声"
          v-model="inputComment"></el-input>
        <div
          style="display: flex; justify-content: flex-end; height: 30px; width: 100%; font-size: 10px; margin-top: 2px; margin-bottom: 10px;">
          <el-button type="danger" size="mini" round icon="el-icon-edit" @click="crazyComment">
            发表评论
          </el-button>
        </div>

      </div>
      <el-row v-for="item in commentData" :key="item.id" style="width: 100%;">
        <el-card style=" height: 3%; margin-top: 6px; font-size: 12px;" shadow="always">

          <div>
            <!--用户头像和用户名 时间等-->
            <div style="height: 30%;  display: flex;">
                <!--头像-->
                <div style="width: 10%; ">
                  <img :src="item.headImageUrl" style="width: 40px; height: 40px; border-radius: 30px; ">
                </div>
                <div style="width: 80%; margin-left: 3px; font-weight: 500; font-size: 12px;">
                  <div style="text-align: left; margin-top: 3px;">{{ item.userName }}</div>
                  <div style="text-align: left; margin-top: 5px; color: #909090;"><span style="font-size: 12px;">{{
                      item.betweenTime }}</span><span style="font-size: 12px; margin-left: 1px;">{{ item.betweenTimeUnit
                      }}</span></div>
                </div>
              </div>

            <!--评论内容-->
            <div style="height: 80%; margin-left: 10%;">
              <p style="display: flex; justify-content: flex-start; align-items: center; font-size: 14px; font-weight: 400;">{{ item.comment }}</p>
              <div style="display: flex; justify-content: flex-end; margin-right: 15px;">
                <el-button icon="el-icon-time" type="text" style="border: none; cursor: default; color: black; font-size: 12px; font-weight: 400;"
                size="medium">
                  {{ item.createTime }}
                </el-button>
              </div>
            </div>
          </div>
        </el-card>

      </el-row>
    </el-drawer>

    <!-- 发疯弹窗 -->
    <el-dialog title="我要发疯" :visible.sync="goCrazyDialog" width="30%" center>
      <el-input type="textarea" :autosize="{ minRows: 3, maxRows: 10 }" placeholder="请尽情的发疯吧"
        v-model="goCrazyContent"></el-input>

      <span slot="footer" class="dialog-footer">
        <el-button @click="goCrazyDialog = false">取 消</el-button>
        <el-button type="primary" @click="goCrazySave">匿名发疯</el-button>
        <el-button type="primary" @click="goCrazySaveReal">实名发疯</el-button>
      </span>
    </el-dialog>

    <!-- 登录 -->
    <el-dialog :visible.sync="loginDialogVisible" width="30%">
      <div style="display: flex; align-items: center; justify-content: center;">
        <el-form ref="userRef" :rules="rules" :model="user" style="width: 80%;">
          <div style="font-size: 20px; font-weight: bold; text-align: center; margin-bottom: 30px;">欢迎登陆我要摸鱼系统
          </div>

          <el-form-item prop="username">
            <el-input prefix-icon="el-icon-user" v-model="user.username" style="font-size: 12px;"
              placeholder="请输入用户名"></el-input>
          </el-form-item>

          <el-form-item prop="password">
            <el-input show-password prefix-icon="el-icon-lock" v-model="user.password" style="font-size: 12px;"
              placeholder="请输入密码"></el-input>
          </el-form-item>
          <el-form-item prop="validCodeInput">
            <div style="display: flex;">
              <el-input prefix-icon="el-icon-circle-check" v-model="user.validCodeInput" size="medium"
                style="flex: 1; font-size: 12px;" placeholder="请输入验证码"></el-input>
              <div style="flex: 1; height: 34px;">
                <valid-code @update:value="getCode"></valid-code>
              </div>
            </div>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" style="width: 100%;" @click="login">登录</el-button>
          </el-form-item>
          <div style="display:flex; font-size: 13px;">
          </div>
        </el-form>
      </div>
    </el-dialog>

  </div>
</template>

<script>
import ValidCode from '@/components/ValidCode.vue';

export default {
  name: 'goCrazy',
  components: {
    ValidCode  
  },
  data() {
    var validCodeRule = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入验证码'));
      } else if (this.validCodeComponent.toLowerCase() !== value.toLowerCase()) {
        callback(new Error('验证码错误！'));
      } else {
        callback();
      }
    };

    return {
      validCodeComponent: '', //验证码组件的值

      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],

        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ],
        validCodeInput: [
          { validator: validCodeRule, trigger: 'blur' }
        ],

      },

      user: {
        id: '',
        password: '',
        username: '',
        phone: '',
        address: '',
        mail: '',
        confirmPassword: ''
      },

      goCrazys: [
        { id: 1, content: '公司的产品和服务质量太差了，简直是在欺骗消费者；管理也是一团糟，让人无法忍受；合作氛围简直是一团糟，让人无法协作完成任务；公司的薪酬和福利简直是无法与同行业公司相比，让人感到失望；公司的晋升渠道完全不透明，让人无法了解自己的职业发展前景；公司的加班情况简直是无法想象的，让人身心俱疲。', readNum: 120, commentNum: 5, starNum: 20, createTime: '2024-8-12', starIcon: 'el-icon-star-off' },
        { id: 2, content: '公司的文化简直就是一团糟，不尊重员工、不尊重客户，毫无企业道德可言；公司的服务质量简直太差了，完全不值得我们继续合作；公司的文化简直就是压迫式的，让员工没有自由发挥的空间；公司的培训和发展机会简直是不堪入目，让人无法获得更好的职业发展；公司的加班文化，简直就是对生命的亵渎，仿佛每一天都走在通向坟墓的大道上。', readNum: 80, commentNum: 10, starNum: 15, createTime: '2024-8-15', starIcon: 'el-icon-star-on' },
      ],
      starIcon: 'el-icon-star-off',
      isStar: false,
      commentData: [
        {
          id: 1,
          comment: '评论1',
          createTime: '2024-7-16'
        },
        {
          id: 2,
          comment: '评论2',
          createTime: '2024-8-12'
        },
        {
          id: 3,
          comment: '评论3',
          createTime: '2024-9-6'
        }
      ],
      drawer: false,
      detailDrawer: false,
      inputComment: '',
      goCrazyDialog: false,
      loginDialogVisible: false,
      goCrazyContent: '',
      keyword: '',
      total: 100,
      currentPage: 1,
      crazyContentId: '',
      detailItem: {}
    }
  },

  methods: {
    getCode(code) {
      this.validCodeComponent = code;
      console.log("验证码组件code:" + code);
    },

    // 查看详情
    viewDetail(item) {
      this.detailItem = item;

      var params = { 'crazyContentId': item.id };
      this.request.post('crazyComment/list', params).then(response => {
        if (response.code != '200') {
          this.$message.error("查看评论失败，请重试！")
        } else {
          //改变图标样式
          let rows = response.data;
          for (var i = 0; i < rows.length; i++) {
            let row = rows[i];
            var minutes = this.calculateTime(row.createTime);
            if (minutes > 60) {
              let hour = Math.floor(minutes / 60)
              if (hour >= 24) {
                let day = Math.floor(hour / 24);
                row.betweenTime = day;
                row.betweenTimeUnit = '天前';
              } else {
                row.betweenTime = hour;
                row.betweenTimeUnit = '小时前';
              }
            } else {
              row.betweenTime = minutes;
              row.betweenTimeUnit = '分钟前';
            }
          }

          this.commentData = rows
        }
      });

      //弹窗
      this.detailDrawer = true;
      this.crazyContentId = item.id;

      //调接口阅读量+1
      this.request.post('crazyContent/read', { "id": item.id }).then(response => {
        if (response.code != '200') {
          this.$message.error("增加阅读量失败")
        } else {
          //暂不做处理
        }
      });
    },

    //点赞
    handleStar(item) {
      // item.starIcon = item.starIcon === 'el-icon-star-off' ? 'el-icon-star-on' : 'el-icon-star-off';
      //TODO 发请求 点赞 or 取消点赞
      var reqUrl = ''
      if (item.starIcon === 'el-icon-star-off') {
        reqUrl = 'crazyContent/star'
      } else {
        reqUrl = 'crazyContent/unstar'
      }
      this.request.post(reqUrl, { 'id': item.id }).then(response => {
        if (response.code != '200') {
          this.$message.error("操作失败，请重试！")
        } else {
          //改变图标样式
          if (item.starIcon === 'el-icon-star-off') {
            item.starIcon = 'el-icon-star-on';
            item.starNum = item.starNum + 1
          } else {
            item.starIcon = 'el-icon-star-off';
            item.starNum = item.starNum - 1;
          }
        }
      });

      //调接口阅读量+1
      this.request.post('crazyContent/read', { "id": item.id }).then(response => {
        if (response.code != '200') {
          this.$message.error("增加阅读量失败")
        } else {
          //暂不做处理
        }
      });

    },

    //查看评论
    comment(item) {
      // this.commentData.title = item.content.substring(0, 30);

      // 发请求查询评论
      var params = { 'crazyContentId': item.id };
      this.request.post('crazyComment/list', params).then(response => {
        if (response.code != '200') {
          this.$message.error("查看评论失败，请重试！")
        } else {
          //改变图标样式
          let rows = response.data;
          for (var i = 0; i < rows.length; i++) {
            let row = rows[i];
            var minutes = this.calculateTime(row.createTime);
            if (minutes > 60) {
              let hour = Math.floor(minutes / 60)
              if (hour >= 24) {
                let day = Math.floor(hour / 24);
                row.betweenTime = day;
                row.betweenTimeUnit = '天前';
              } else {
                row.betweenTime = hour;
                row.betweenTimeUnit = '小时前';
              }
            } else {
              row.betweenTime = minutes;
              row.betweenTimeUnit = '分钟前';
            }
          }

          this.commentData = rows
        }
      });

      //弹窗
      this.drawer = true;
      this.crazyContentId = item.id;

      //调接口阅读量+1
      this.request.post('crazyContent/read', { "id": item.id }).then(response => {
        if (response.code != '200') {
          this.$message.error("增加阅读量失败")
        } else {
          //暂不做处理
        }
      });
    },

    calculateTime(startTime) {
      var start = Date.parse(startTime);
      var end = new Date();
      var diff = Math.abs(end - start);
      var minutes = Math.floor(diff / (60 * 1000))

      return minutes;
    },

    //TODO 查询发疯内容
    search() {
      this.request.post('crazyContent/list', { 'keyword': this.keyword, 'page': 1, "rows": 20 }).then(response => {
        if (response.code != '200') {
          this.$message.error("查询打工人发疯内容失败，请重试！")
        } else {
          this.total = response.data.total;
          let rows = response.data.rows;

          for (var i = 0; i < rows.length; i++) {
            let row = rows[i];
            var minutes = this.calculateTime(row.createTime);
            if (minutes > 60) {
              let hour = Math.floor(minutes / 60)
              if (hour >= 24) {
                let day = Math.floor(hour / 24);
                row.betweenTime = day;
                row.betweenTimeUnit = '天前';
              } else {
                row.betweenTime = hour;
                row.betweenTimeUnit = '小时前';
              }
            } else {
              row.betweenTime = minutes;
              row.betweenTimeUnit = '分钟前';
            }
          }

          console.log(rows)
          this.goCrazys = rows;
        }
      });
    },

    myComment() {
      let token = localStorage.getItem('moyu_token')
      if (token == null) {
        this.$message.warn("用户未登录，查询全部！")
        this.search()
      } else {
        let praseToken = JSON.parse(token)

        this.request.post('crazyContent/list', { 'keyword': this.keyword, 'page': 1, "rows": 20, 'userId': praseToken.id }).then(response => {
          if (response.code != '200') {
            this.$message.error("查询我的发帖失败，请重试！")
          } else {
            this.total = response.data.total;
            this.goCrazys = response.data.rows;
          }
        });
      }
    },

    //列表翻页
    handleCurrentChange(val) {
      this.request.post('crazyContent/list', { 'keyword': this.keyword, 'page': val, "rows": 20 }).then(response => {
        if (response.code != '200') {
          this.$message.error("查询打工人发疯内容失败，请重试！")
        } else {
          this.total = response.data.total;

          let rows = response.data.rows;
          for (var i = 0; i < rows.length; i++) {
            let row = rows[i];
            var minutes = this.calculateTime(row.createTime);
            if (minutes > 60) {
              let hour = Math.floor(minutes / 60)
              if (hour >= 24) {
                let day = Math.floor(hour / 24);
                row.betweenTime = day;
                row.betweenTimeUnit = '天前';
              } else {
                row.betweenTime = hour;
                row.betweenTimeUnit = '小时前';
              }
            } else {
              row.betweenTime = minutes;
              row.betweenTimeUnit = '分钟前';
            }
          }

          this.goCrazys = rows;
        }
      });
    },

    //匿名发疯保存
    goCrazySave() {
      this.request.post('crazyContent/save', { 'content': this.goCrazyContent }).then(response => {
        if (response.code != '200') {
          this.$message.error("发疯失败，请重试！")
        } else {
          this.goCrazyDialog = false;
          this.goCrazyContent = '';
          this.search();
        }
      });

    },

    //实名发疯保存
    goCrazySaveReal() {
      let token = localStorage.getItem("moyu_token");
      if (token == null) {
        this.loginDialogVisible = true;
      } else {
        let parseToken = JSON.parse(token)
        this.request.post('crazyContent/save', { 'content': this.goCrazyContent, 'userId': parseToken.id }).then(response => {
          if (response.code != '200') {
            this.$message.error("发疯失败，请重试！")
          } else {
            this.goCrazyDialog = false;
            this.goCrazyContent = '';
            this.search();
          }
        });
      }
    },

    login() {
      this.$refs['userRef'].validate((valid) => {
        if (valid) {
          this.request.post("user/login", { 'username': this.user.username, 'password': this.user.password }).then(response => {
            if (response.code != '200') {
              this.$message.error(response.message)
            } else {
              this.loginDialogVisible = false;
              let respData = response.data
              localStorage.setItem("moyu_token", JSON.stringify(respData))
              window.location.reload()
            }
          })
        }
      });
    },

    //评论保存
    crazyComment() {
      this.request.post('crazyComment/save', { 'comment': this.inputComment, 'crazyContentId': this.crazyContentId }).then(response => {
        if (response.code != '200') {
          this.$message.error("发表评论失败，请重试！")
        } else {
          this.inputComment = '';
          var params = { 'crazyContentId': item.id };
          this.request.post('crazyComment/list', params).then(response => {
            if (response.code != '200') {
              this.$message.error("查看评论失败，请重试！")
            } else {
              //改变图标样式
              this.commentData = response.data
            }
          });
        }
      });
    }

  },

  mounted() {
    this.search();

  }
}
</script>

<style scoped></style>