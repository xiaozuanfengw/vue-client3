<template>
    <section class="msite">
        <!--首页头部-->
        <Header :title="address.name || '正在定位中'">
          <span class="header_search" slot="left">
            <i class="iconfont icon-sousuo"></i>
          </span>
          <span class="header_login" slot="right">
            <span class="header_login_text">登录|注册</span>
          </span>
        </Header>
        <!--首页导航-->
        <nav class="msite_nav">
          <div class="swiper-container">
            <div class="swiper-wrapper">
               <div class="swiper-slide" v-for="(categorys, index) in categorysArr" :key="index">
                  <a href="javascript:" class="link_to_food" v-for="(c, index) in categorys" :key="index">
                    <div class="food_container">
                      <img :src="'https://fuss10.elemecdn.com' + c.image_url">
                    </div>
                    <span>{{c.title}}</span>
                  </a>
                <!-- <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/2.jpg">
                  </div>
                  <span>商超便利</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/3.jpg">
                  </div>
                  <span>美食</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/4.jpg">
                  </div>
                  <span>简餐</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/5.jpg">
                  </div>
                  <span>新店特惠</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/6.jpg">
                  </div>
                  <span>准时达</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/7.jpg">
                  </div>
                  <span>预订早餐</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/8.jpg">
                  </div>
                  <span>土豪推荐</span>
                </a> -->
              </div>
              <!-- <div class="swiper-slide">
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/9.jpg">
                  </div>
                  <span>甜品饮品</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/10.jpg">
                  </div>
                  <span>商超便利</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/11.jpg">
                  </div>
                  <span>美食</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/12.jpg">
                  </div>
                  <span>简餐</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/13.jpg">
                  </div>
                  <span>新店特惠</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/14.jpg">
                  </div>
                  <span>准时达</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/1.jpg">
                  </div>
                  <span>预订早餐</span>
                </a>
                <a href="javascript:" class="link_to_food">
                  <div class="food_container">
                    <img src="./images/nav/2.jpg">
                  </div>
                  <span>土豪推荐</span>
                </a>
              </div> -->
            </div>
            <!-- Add Pagination -->
            <div class="swiper-pagination"></div>
          </div>
        </nav>
        <!--首页附近商家-->
        <ShopList/>
      </section>
</template>

<script>
import Swiper from 'swiper'
import 'swiper/dist/css/swiper.min.css'
import ShopList from '../../components/ShopList/ShopList'
import { mapState } from 'vuex'


    export default {
      name: "msite",
      computed: {
        ...mapState(['address','categorys']),
        categorysArr () {
        // 取出相关的数据
        const bigArr = []
        let smallArr = []
        const {categorys} = this
        // 计算产生结果
        categorys.forEach(c => {

          // 将小数组放入大数组(同一个小数组只能被保存一次)
          if (smallArr.length===0) {
            bigArr.push(smallArr)
          }

          // 将分类对象放入小数组(小数组的长度最大为8)
          smallArr.push(c)
          // 如果满了, 重新准备一个新的小数组
          if (smallArr.length===8) {
            smallArr = []
          }

        })

        // 返回结果
        return bigArr
      }
      },
      mounted() {

         this.$store.dispatch("getShops")
         this.$store.dispatch("getCategorys")

        // var mySwiper = new Swiper ('.swiper-container', {
        //   // direction: 'vertical', // 垂直切换选项  默认是水平滑动
        //   loop: true, // 循环模式选项
        //   // 如果需要分页器
        //   pagination: {
        //     el: '.swiper-pagination',
        //   },
        // })
      },
 watch: {
      // 更新状态数据 ==> 立即同步调用监视的回调函数 ==> 异步更新界面
      categorys () { // categorys状态数据更新了
        // 将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。
        this.$nextTick(() => { // 回调函数在界面更新之后执行
          new Swiper ('.swiper-container', {
            // direction: 'vertical', // 垂直切换选项
            loop: true, // 循环模式选项
            // 如果需要分页器
            pagination: {
              el: '.swiper-pagination',
            },
          })
        })
      }
    },



        components:{
          ShopList
        }
    }
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
@import '../../common/stylus/mixins.styl'
  .msite  //首页
    width 100%
    .msite_nav
      bottom-border-1px(#e4e4e4)
      margin-top 45px
      height 200px
      background #fff
      .swiper-container
        width 100%
        height 100%
        .swiper-wrapper
          width 100%
          height 100%
          .swiper-slide
            display flex
            justify-content center
            align-items flex-start
            flex-wrap wrap
            .link_to_food
              width 25%
              .food_container
                display block
                width 100%
                text-align center
                padding-bottom 10px
                font-size 0
                img
                  display inline-block
                  width 50px
                  height 50px
              span
                display block
                width 100%
                text-align center
                font-size 13px
                color #666
        .swiper-pagination
          >span.swiper-pagination-bullet-active
            background #02a774
    </style>
