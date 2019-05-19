<template>
  <div>
    <!--搜索条-->
    <search-bar></search-bar>
    <!--轮播图-->
    <swiper :indicator-dots="indicatorDots">
      <swiper-item :key="index" v-for="(item,index) in imgUrls">
        <image :src="item" class="slide-image">
      </swiper-item>
    </swiper>
    <div class="menu">
      <img src="/static/images/user.png" alt>
      <img src="/static/images/user.png" alt>
      <img src="/static/images/user.png" alt>
      <img src="/static/images/user.png" alt>
    </div>
  </div>
</template>

<script>
// 导入公共的search组件
import SearchBar from "../../components/search";
export default {
  data() {
    return {
      // 该属性的作用：显示底部的小圆圈
      indicatorDots: true,
      imgUrls: [
        "https://images.unsplash.com/photo-1551334787-21e6bd3ab135?w=640",
        "https://images.unsplash.com/photo-1551214012-84f95e060dee?w=640",
        "https://images.unsplash.com/photo-1551446591-142875a901a1?w=640"
      ]
    };
  },
  components: {
    "search-bar": SearchBar
  },
  created() {
    let that = this;
    // 调用后台接口，获取轮播图数据
    mpvue.request({
      url: "https://www.zhengzhicheng.cn/api/public/v1/home/swiperdata",
      success: function(res) {
        // console.log(res)
        // 使用服务器返回的数据覆盖data中的数据
        // that.imgUrls = res.data.message
        // res.data.message本身是对象数组，但是我们只需要图片地址
        let imgs = res.data.message.map(item => {
          return item.image_src;
        });
        // 更新图片数据
        that.imgUrls = imgs;
      }
    });
  }
};
</script>

<style scoped>
.slide-image {
  width: 750rpx;
}
.menu {
  display: flex;
  justify-content: space-around;
}
.menu img {
  width: 128rpx;
  height: 140rpx;
}
</style>
