<template>
  <div class="comment-list">
    <div class="page-title" v-if="comments.length">
      评论
    </div>
    <div class="comment" v-for="(comment, index) in comments" :key="index" @click="handleClick(comment)">
      <div class="user">
        <div class="inline">
          <img :src="comment.image" class="avatar" mode="aspectFit">
          {{comment.title}}
        </div>
        <div class="right">
          {{comment.location || '未知地点' }}
          <span class="text-primary">--</span>
          {{comment.phone || '未知型号'}}
        </div>
      </div>
      <div class="content">
        {{comment.comment}}
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    props: {
      comments: {
        type: Array,
        require: true
      },
      type: {
        type: String,
        default: ''
      }
    },
    methods: {
      handleClick (comment) {
        if (this.type === 'user') {
          wx.navigateTo({
            url: '/pages/detail/main?id=' + comment.bookid
          })
        }
      }
    }
  }
</script>
<style lang="less" scoped>
  .comment-list {
    background: #eee;
    font-size: 14px;

    .comment {
      background: #FFF;
      margin-bottom: 10px;
      padding: 5px 20px;

      .user {
        .inline{
          display: inline;
        }
        .avatar {
          width: 20px;
          height: 20px;
          border-radius: 50%;
          vertical-align: middle;
        }
      }

      .content {
        margin: 10px 0px
      }
    }

  }

</style>
