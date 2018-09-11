<template>
  <div class="swiper">
    <swiper
      :indicator-dots="true"
      indircator-color="#EA5A49"
      :autoplay="true"
      :interval="6000"
      :duration="1000"
      :circular="true"
    >
    <div v-for="(top, imgindex) in imgUrl" :key="imgindex">
      <swiper-item>
        <img
         @click="bookDetail(img)"
         class="slide-image"
         mode="aspectFit"
         v-for="(img, i) in top" :key="i"
        :src="img.image">
      </swiper-item>
    </div>
    </swiper>
  </div>
</template>
<script>
export default {
  props: ['tops'],
  computed: {
    imgUrl () {
      let res = this.tops
      return [res.slice(0, 3), res.slice(3, 6), res.slice(6)]
    }
  },
  methods: {
    bookDetail (item) {
      wx.navigateTo({
        url: '/pages/detail/main?id=' + item.id
      })
    }
  }
}
</script>
<style lang="less" scoped>
 .swiper{
   margin-top: 5px;
   .slide-image{
     width:33%;
     height: 250rpx;
   }
 }
</style>
