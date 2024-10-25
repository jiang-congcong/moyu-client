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
          <p style="display: flex; justify-content: flex-start; cursor: pointer;" @click="viewDetail(item)">
            {{ item.content.length > 70 ? item.content.substring(0, 70) + '...' : item.content }}
          </p>
          <div style="display: flex; justify-content: flex-start;">
            <el-button :class="item.starIcon" @click="handleStar(item)" style="border: none;" size="medium">
              {{ item.starNum }}
            </el-button>
            <el-button icon="el-icon-s-comment" @click="comment(item)" style="border: none;" size="medium">
              {{ item.commentNum }}
            </el-button>
            <el-button icon="el-icon-view" type="text"
              style="border: none; cursor: default; color: black; margin-left: 20px;" size="medium">
              {{ item.readNum }}
            </el-button>
            <el-button icon="el-icon-time" type="text"
              style="border: none; cursor: default; color: black; margin-left: 30px;" size="medium">
              {{ item.createTime }}
            </el-button>
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
          <p style="display: flex; justify-content: flex-start; align-items: center;">{{ item.comment }}</p>
          <div style="display: flex; justify-content: flex-end; margin-right: 15px;">
            <el-button icon="el-icon-time" type="text" style="border: none; cursor: default; color: black;"
              size="medium">
              {{ item.createTime }}
            </el-button>
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
          <p style="display: flex; justify-content: flex-start; align-items: center;">{{ item.comment }}</p>
          <div style="display: flex; justify-content: flex-end; margin-right: 15px;">
            <el-button icon="el-icon-time" type="text" style="border: none; cursor: default; color: black;"
              size="medium">
              {{ item.createTime }}
            </el-button>
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
        <el-button type="primary" @click="goCrazySave">确 定</el-button>
      </span>
    </el-dialog>

  </div>
</template>

<script>
export default {
  name: 'goCrazy',
  data() {
    return {
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
      goCrazyContent: '',
      keyword: '',
      total: 100,
      currentPage: 1,
      crazyContentId: '',
      detailItem: {}
    }
  },

  methods: {
    //TODO 查看详情
    viewDetail(item) {
      this.detailItem = item;

      var params = { 'crazyContentId': item.id };
      this.request.post('crazyComment/list', params).then(response => {
        if (response.code != '200') {
          this.$message.error("查看评论失败，请重试！")
        } else {
          //改变图标样式
          this.commentData = response.data
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
          this.commentData = response.data
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

    //TODO 查询发疯内容
    search() {
      this.request.post('crazyContent/list', { 'keyword': this.keyword, 'page': 1, "rows": 20 }).then(response => {
        if (response.code != '200') {
          this.$message.error("查询打工人发疯内容失败，请重试！")
        } else {
          this.total = response.data.total;
          this.goCrazys = response.data.rows;
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

        this.request.post('crazyContent/list', { 'keyword': this.keyword, 'page': 1, "rows": 20, 'userId': praseToken.id}).then(response => {
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
          this.goCrazys = response.data.rows;
        }
      });
    },

    //发疯保存
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