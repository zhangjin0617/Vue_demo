<template>
    <section class="msite">
        <!--首页头部-->
        <Header :title="address.name||'正在定位中...'">
          <span class="header_search" slot="left">
            <i class="iconfont icon-sousuo"></i>
          </span>
          <span class="header_login" slot="right">
            <span class="header_login_text">登录|注册</span>
          </span>
        </Header>
        <!--首页导航-->
         <!-- <nav class="msite_nav">
          <div class="swiper-container">
            <div class="swiper-wrapper">

              <div class="swiper-slide" v-for="(categorys,index)in categorysArr" :key="index">
                <a href="javascript:" class="link_to_food" v-for="(c,index) in categorys" :key="index">
                  <div class="food_container">
                    <img :src="'https://fuss10.elemecdn.com' + c.image_url">
                  </div>
                  <span>{{c.title}}</span>
                </a>
              </div>
            </div>
            <!-- Add Pagination -->
            <div class="swiper-pagination"></div>
          </div>
        <!-- </!--> --> -->
        <nav class="msite_nav">
      <div class="swiper-container">
        <div class="swiper-wrapper">
          <!-- categorysArr -->
          <div class="swiper-slide" v-for="(categorys, index) in categorysArr" :key="index">
            <a href="javascript:" class="link_to_food" v-for="(c, index) in categorys" :key="index">
              <div class="food_container">
                <img :src="'https://fuss10.elemecdn.com' + c.image_url">
              </div>
              <span>{{c.title}}</span>
            </a>
          </div>

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
import {mapState} from 'vuex'

    export default {
        name: "msite",
        computed:{
          ...mapState(['address','categorys']),
          categorysArr(){
            const bigArr=[]
            let smallArr=[]
            const {categorys}=this
            categorys.forEach(c => {
              if(smallArr.length===0){
                  bigArr.push(smallArr)
              }
              smallArr.push(c)
                if(smallArr.length===8){
                  smallArr=[]
                }

            })
              return bigArr
          }
        },


        async mounted() {
          this.$store.dispatch("getShops")
          await this.$store.dispatch("getCategorys")
          new Swiper ('.swiper-container', {
            // direction: 'vertical', // 垂直切换选项
            loop: true, // 循环模式选项

            // 如果需要分页器
            pagination: {
              el: '.swiper-pagination',
            },

})
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
