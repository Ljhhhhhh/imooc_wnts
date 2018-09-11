<template>
  <div>
    <BookInfo :info="info"></BookInfo>
    <CommentList :comments="comments"></CommentList>
    <div class="comment" v-if="showAdd">
      <textarea v-model="comment" class="textarea" :maxlength="100" placeholder="请输入图书评论短评">
     </textarea>
      <div class="location">
        地理位置：
        <switch color="#EA5A49" :checked="location" @change="getGeo"></switch>
        <span class="text-primary">{{location}}</span>
      </div>
      <div class="phone">
        手机型号：
        <switch color="#EA5A49" :checked="phone" @change="getPhone"></switch>
        <span class="text-primary">{{phone}}</span>
      </div>
      <button class="btn" @click="addComment">评论</button>
    </div>
    <div v-else class="text-footer">
      未登陆或者已经评论过啦
    </div>
    <button class="btn" open-type="share">转发给</button>
  </div>
</template>
<script>
  import {
    get,
    post,
    showModal
  } from '@/util'
  import BookInfo from '@/components/BookInfo'
  import CommentList from '@/components/CommentList'
  export default {
    data () {
      return {
        bookid: '',
        info: {},
        comment: '',
        location: '',
        phone: '',
        userinfo: {},
        comments: []
      }
    },
    mounted () {
      this.bookid = this.$root.$mp.query.id
      this.getDetail()
      this.getComments()
      const userinfo = wx.getStorageSync('userinfo')
      if (userinfo) {
        this.userinfo = userinfo
      }
    },
    computed: {
      showAdd () {
        if (!this.userinfo.openId) {
          return false
        }
        if (this.comments.filter(v => v.openid === this.userinfo.openId).length) {
          return false
        }
        return true
      }
    },
    methods: {
      async getDetail () {
        const info = await get('/weapp/bookdetail', {
          id: this.bookid
        })
        wx.setNavigationBarTitle({
          title: info.title
        })
        this.info = info
      },
      async getComments () {
        const comments = await get('/weapp/commentlist', {bookid: this.bookid})
        this.comments = comments.list || []
      },
      getGeo (e) {
        const ak = 'LaEoymfa6YakB05cejO84LIhlFUsYGLr'
        let url = 'http://api.map.baidu.com/geocoder/v2/'
        if (e.target.value) {
          wx.getLocation({
            success: geo => {
              wx.request({
                url,
                data: {
                  ak,
                  location: `${geo.latitude},${get.longitude}`,
                  output: 'json'
                },
                success: res => {
                  if (res.data.status === 0) {
                    this.location = res.data.result.addressComponent.city
                  } else {
                    this.location = '未知地点'
                    console.log('获取位置出错')
                  }
                }
              })
            }
          })
        }
      },
      getPhone (e) {
        if (e.target.value) {
          const phoneInfo = wx.getSystemInfoSync()
          this.phone = phoneInfo.model
        } else {
          this.phone = ''
        }
      },
      async addComment () {
        if (!this.comment) {
          return
        }
        const data = {
          bookid: this.bookid,
          comment: this.comment,
          phone: this.phone,
          location: this.location,
          openid: this.userinfo.openId
        }
        try {
          await post('/weapp/addcomment', data)
          this.comment = ''
          this.getComments()
        } catch (e) {
          showModal('失败', e.msg)
        }
      }
    },
    components: {
      BookInfo,
      CommentList
    }
  }
</script>
<style lang="less" scoped>
  .comment {
    margin-top: 10px;

    .textarea {
      width: 730rpx;
      height: 200rpx;
      background: #eee;
      padding: 10px
    }

    .location,
    .phone {
      margin-top: 10px;
      padding: 5px 10px;
    }
  }

</style>
