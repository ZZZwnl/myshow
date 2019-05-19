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
    <!--楼层效果-->
    <div v-for='(item,index) in getImgs' :key='index' class="floor">
      <div class="floor-title">
        <img :src="item.floor_title.image_src" alt="">
      </div>
      <div class="floor-content">
        <div class="left">
          <!-- <img :src="item.product_list[0].image_src" alt=""> -->
          <img :src="item.leftImg.image_src" alt="">
        </div>
        <div class="right">
          <img v-for='(img,i) in item.product_list' :key='i' :src="img.image_src" alt="">
        </div>
      </div>
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
      menus: [],
      floors:[]
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
    this.imgUrls = imgs

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
    this.menus = menuRes.data.message

    // 获取楼层数据
    let floorsRes = await request('home/floordata')
    this.floors = floorsRes.data.message
  },
  computed: {
    getImgs(){
      // 加工楼层数据:必须最后返回加工之后的结果
      // 计算属性依赖于data中的数据
      return this.floors.map(item=>{
        // 过滤出图片的后四项数据
        let imgs = item.product_list.filter((img,index)=>{
          return index > 0
        })
        // 添加一条数据保存左侧图片
        item.leftImg = item.product_list[0]
        // 覆盖默认的5张图片
        item.product_list = imgs
        // 把加工之后的数据返回
        return item
      })
    }
  }
}
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
.floor {
  margin-top: 20rpx;
}
.floor .floor-title img{
  width: 750rpx;
  height: 80rpx;
}
.floor .floor-content{
  display: flex;
  justify-content: space-around;
}
.floor .floor-content .left img{
  width: 260rpx;
  height: 370rpx;
}
.floor .floor-content .right{
  flex: 1;
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}
.floor .floor-content .right img{
  width: 232rpx;
  height: 188rpx;
  border-radius: 4rpx;
}
</style>
