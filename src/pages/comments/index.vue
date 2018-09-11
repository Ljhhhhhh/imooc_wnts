<template>
  <div class="container">
    <CommentList v-if="userinfo.openId" type="user" :comments="comments"></CommentList>
    <div v-if="userinfo.openId">
      <div class="page-title">我的图书</div>
      <Card v-for="book in books" :key="book.id" :book="book"></Card>
      <div v-if="!books.length">暂时还没有添加图书</div>
    </div>
  </div>
</template>
<script>
import Card from '@/components/Card'
import CommentList from '@/components/CommentList'
import {get} from '@/util'
export default {
  data () {
    return {
      comments: [],
      userinfo: {},
      books: []
    }
  },
  onPullDownRefresh () {
    this.init()
    wx.stopPullDownRefresh()
  },
  onShow () {
    if (!this.userinfo.openId) {
      let userinfo = wx.getStorageSync('userinfo')
      if (userinfo) {
        this.userinfo = userinfo
        this.init()
      }
    }
  },
  methods: {
    init () {
      wx.showNavigationBarLoading()
      this.getComments()
      this.getBooks()
      wx.hideNavigationBarLoading()
    },
    async getBooks () {
      const books = await get('/weapp/booklist', {
        openid: this.userinfo.openId
      })
      this.books = books.list
    },
    async getComments () {
      const comments = await get('/weapp/commentlist', {
        openid: this.userinfo.openId
      })
      this.comments = comments.list
      console.log(comments)
    }
  },
  components: {
    CommentList,
    Card
  }
}
</script>

