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
    <!--回到顶部-->
    <div v-if='isShow' @click="toTopHandle" class="to-top">
      ︿
      <p>顶部</p>
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
      isShow:false,
      // 该属性的作用：显示底部的小圆圈
      indicatorDots: true,
      imgUrls: [],
      menus: [],
      floors:[]
    }
  },
  methods: {
    toTopHandle(){
      // 控制回到顶部
      mpvue.pageScrollTo({
        scrollTop: 0
      })
    },
    async initData(){
      // 轮播图数据
      let swiperRes = await request("home/swiperdata")
      let imgs = swiperRes.data.message.map(item => {
        return item.image_src
      })
      this.imgUrls = imgs
      // 菜单数据
      let menuRes = await request('home/catitems')
      this.menus = menuRes.data.message
      // 楼层数据
      let floorsRes = await request('home/floordata')
      this.floors = floorsRes.data.message
    }
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

    // let swiperRes = await request("home/swiperdata");
    // let imgs = swiperRes.data.message.map(item => {
    //   return item.image_src
    // })
    // this.imgUrls = imgs

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

    // let menuRes = await request('home/catitems')
    // this.menus = menuRes.data.message

    // 获取楼层数据
    // let floorsRes = await request('home/floordata')
    // this.floors = floorsRes.data.message

    this.initData()
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
  },
  onPageScroll(event){
    // console.log(event.scrollTop)
    // 当滚动条大于指定值的时候，显示回到顶部的按钮
    this.isShow = event.scrollTop > 50
  },
  onPullDownRefresh () {
  // 下拉刷新，重新加载页面的数据
  this.initData()
}
}
</script>

<style scoped lang='scss'>
  @import 'main.scss'
</style>
