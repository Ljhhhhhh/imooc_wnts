<template>
  <div class="container">
    <p>{{userinfo.openId}}</p>
    <div class="userinfo">
      <img :src="userinfo.avatarUrl" alt="">
      <button class="login_btn" open-type="getUserInfo" @click="login">{{userinfo.nickName}}</button>
    </div>
    <YearProgress></YearProgress>
    <button v-if="userinfo.openId" class="btn" @click="scanBook">添加图书</button>
  </div>
</template>
<script>
  import {
    showSuccess,
    post,
    showModal
  } from '@/util'
  import qcloud from 'wafer2-client-sdk'
  import config from '@/config'
  import YearProgress from '@/components/YearProgress'
  export default {
    data () {
      return {
        userinfo: {
          avatarUrl: '../../../static/img/unlogin.png',
          nickName: '点击登录'
        }
      }
    },
    created () {
      let userinfo = wx.getStorageSync('userinfo')
      if (userinfo) {
        this.userinfo = userinfo
      }
    },
    onshow () {
      console.log('onshow')
      let userinfo = wx.getStorageSync('userinfo')
      if (userinfo) {
        this.userinfo = userinfo
      }
    },
    methods: {
      async addBook (isbn) {
        const res = await post('/weapp/addbook', {
          isbn,
          openid: this.userinfo.openId
        })
        console.log(res)
        showModal('添加成功', `${res.title}添加成功`)
      },
      scanBook () {
        wx.scanCode({
          success: (res) => {
            if (res.result) {
              this.addBook(res.result)
            }
          }
        })
      },
      login () {
        let user = wx.getStorageSync('userinfo')
        if (!user) {
          qcloud.setLoginUrl(config.loginUrl)
          qcloud.login({
            success: (userinfo) => {
              qcloud.request({
                url: config.userUrl,
                login: true,
                success: (userRes) => {
                  showSuccess('登录成功')
                  console.log(userRes)
                  wx.setStorageSync('userinfo', userRes.data.data)
                  this.userinfo = userRes.data.data
                }
              })
            },
            fail: (err) => {
              console.log('登录失败', err)
            }
          })
        } else {
          this.userinfo = user
        }
      }
    },
    components: {
      YearProgress
    }
  }
</script>

<style lang="less" scoped>
  .container {
    padding: 0 30rpx;

    .userinfo {
      margin-top: 100rpx;
      text-align: center;

      .login_btn {
        border: none;
        background: transparent;
        outline: none;
        line-height: 30px;
        position: inherit;
      }

      img {
        width: 150rpx;
        height: 150rpx;
        margin: 20rpx;
        border-radius: 50%;
      }
    }
  }

</style>
