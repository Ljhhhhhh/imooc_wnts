<template>
  <div class="bookinfo">
    <div class="thumb">
      <img class="back" :src="info.image" mode="aspectFill">
      <img :src="info.image" class="img" mode="aspectFit">
      <div class="info">
        <div class="title">{{info.title}}</div>
        <div class="author">{{info.author}}</div>
      </div>
    </div>
    <div class="detail">
      <img :src="userinfo.avatarUrl" mode="aspectFit" class="avatar">
      {{userinfo.name}}
      <div class="right text-primary">{{info.rate}}
        <Rate :value="info.rate"></Rate>
      </div>
    </div>
    <div class="detail">
      {{info.publisher}}
      <div class="right">
        {{info.price}}
      </div>
    </div>
    <div class="tags">
      <div class="badge" v-for="tag in info.tags" :key="tag">{{tag}}</div>
    </div>
    <div class="summary">
      <p :key="i" v-for="(sum, i) in info.summary">{{sum}}</p>
    </div>
  </div>
</template>
<script>
  import Rate from '@/components/Rate'
  export default {
    props: ['info'],
    computed: {
      userinfo () {
        return this.info.userinfo || {}
      }
    },
    components: {
      Rate
    }
  }
</script>
<style lang="less" scoped>
  .right {
    float: right;
  }

  .detail {
    padding: 5px 10px;
    .avatar {
      width: 20px;
      height: 20px;
      border-radius: 50%;
      vertical-align: middle;
    }
  }

  .bookinfo {
    font-size: 14px;
    .badge{
      display: inline-block;
      margin:5px;
      padding:5px;
      border-radius: 10px;
      border:1px currentColor solid;
      color: #EA5A49;
    }
    .summary{
      padding: 0 15px;
      margin-top:10px;
      p{
        text-indent: 2em;
        font-size: 14px;
      }
    }
    .thumb {
      width: 750rpx;
      height: 500rpx;
      overflow: hidden;
      position: relative;

      .back {
        filter: blur(15px);
        width: 100%;
      }

      .img {
        position: absolute;
        height: 300rpx;
        left: 0rpx;
        top: 30rpx;
      }

      .info {
        columns: #FFF;
        position: absolute;
        width: 100%;
        left: 0;
        top: 330rpx;
        text-align: center;

        .title {
          font-size: 20px;
        }

        .author {
          font-size: 14px;
        }
      }
    }
  }

</style>
