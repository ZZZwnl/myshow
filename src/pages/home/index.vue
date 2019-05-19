<template>
  <div>
    <!--搜索条-->
    <search-bar></search-bar>
    <!--轮播图-->
    <swiper :indicator-dots="indicatorDots">
      <swiper-item :key="index" v-for="(item,index) in imgUrls">
        <image :src="item" class="slide-image"></image>
      </swiper-item>
    </swiper>
    <div class="menu">
      <img v-for="(item,index) in menus" :key="index" :src="item.image_src" alt>
    </div>
  </div>
</template>

<script>
// 导入公共的search组件
import SearchBar from "../../components/search";

// 导入公共方法
import request from "../../utils/request.js";

export default {
  data() {
    return {
      // 该属性的作用：显示底部的小圆圈
      indicatorDots: true,
      imgUrls: [],
      menus: []
    };
  },
  components: {
    "search-bar": SearchBar
  },
  async created() {
    let that = this;
    // 调用后台接口，获取轮播图数据
    // mpvue.request({
    //   url: "https://www.zhengzhicheng.cn/api/public/v1/home/swiperdata",
    //   success: function(res) {
    //     // console.log(res)
    //     // 使用服务器返回的数据覆盖data中的数据
    //     // that.imgUrls = res.data.message
    //     // res.data.message本身是对象数组，但是我们只需要图片地址
    //     let imgs = res.data.message.map(item => {
    //       return item.image_src;
    //     });
    //     // 更新图片数据
    //     that.imgUrls = imgs;
    //   }
    // })

    // request('home/swiperdata')
    //   .then(res=>{
    //     let imgs = res.data.message.map(item => {
    //       return item.image_src;
    //     })
    //     that.imgUrls = imgs;
    // })

    let swiperRes = await request("home/swiperdata");
    let imgs = swiperRes.data.message.map(item => {
      return item.image_src
    })
    that.imgUrls = imgs

    // 调用菜单接口
    // mpvue.request({
    //     url:'https://www.zhengzhicheng.cn/api/public/v1/home/catitems',
    //     success:function(res){
    //         // console.log(res)
    //         // 更新页面数据
    //         that.menus = res.data.message
    //     }
    // })


    // request("home/catitems").then(res => {
    //   that.menus = res.data.message;
    // });

    let menuRes = await request('home/catitems')
    that.menus = menuRes.data.message
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
