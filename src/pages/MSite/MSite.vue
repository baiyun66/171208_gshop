<template>
  <section class="msite">
    <!--首页头部-->
    <HeaderTop :title="address.name">
      <span class="header_search" slot="left">
            <i class="iconfont icon-sousuo"></i>
          </span>
      <span class="header_login" slot="right">
            <span class="header_login_text">登录|注册</span>
          </span>
    </HeaderTop>
    <!--首页导航-->
    <nav class="msite_nav">
      <div class="swiper-container" v-if="categorys.length">
        <div class="swiper-wrapper">
          <div class="swiper-slide" v-for="(categorys,index) in CategoryArray" :key="index">
            <a href="javascript:" class="link_to_food" v-for="(category,index) in categorys" :key="index">
              <div class="food_container">
                <img :src="imgBaseUrl+category.image_url">
              </div>
              <span>{{category.title}}</span>
            </a>
          </div>
        </div>
        <!-- Add Pagination -->
        <div class="swiper-pagination"></div>
      </div>
      <img src="./images/msite_back.svg" v-else>
    </nav>
    <!--首页附近商家-->
    <div class="msite_shop_list">
      <div class="shop_header">
        <i class="iconfont icon-xuanxiang"></i>
        <span class="shop_header_title">附近商家</span>
      </div>
      <ShopList></ShopList>
    </div>
  </section>
</template>
<script>
  import {mapState} from 'vuex'
  import ShopList from '../../components/ShopList/ShopList'
  import HeaderTop from '../../components/HeaderTop/HeaderTop'
  import Swiper from 'swiper'
  import 'swiper/swiper-bundle.min.css'
  import { Swiper as SwiperClass, Pagination, Mousewheel, Autoplay, Navigation ,EffectFade,EffectCoverflow} from 'swiper/core'
  SwiperClass.use([Pagination, Mousewheel, Autoplay,Navigation,EffectFade,EffectCoverflow])


  export default {
    data () {
      return {
        imgBaseUrl: 'https://fuss10.elemecdn.com'
      }
    },
    mounted () {
      this.$store.dispatch('getCategorys')
      this.$store.dispatch('getShops')
      new Swiper('.swiper-container', {
        // 如果需要分页器
        pagination:{
          el:'.swiper-pagination',
          clickable:true,
        },//这样写小圆点就有了
        //设置无缝循环
        loop: true
      })
    },
    computed:{
      ...mapState(['address','categorys','shops']),

      CategoryArray(){
        const {categorys} = this
        let arr2 = []
        let arr1 = []

        categorys.forEach(c=>{
          arr1.push(c)
          if (arr1.length===8)
          {
            arr2.push(arr1)
            arr1 = []
          }
        })
        console.log(arr2)
        return arr2
      }
    },
    watch:{
      categorys(value){
        this.$nextTick(() => {
          new Swiper('.swiper-container', {
              pagination: {
                el: '.swiper-pagination',
              },
            loop: true
          })
        })
      }
    },
    components:{
      HeaderTop,
      ShopList
    }

  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixins.styl"
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
    .msite_shop_list
      top-border-1px(#e4e4e4)
      margin-top 10px
      background #fff
      .shop_header
        padding 10px 10px 0
        .shop_icon
          margin-left 5px
          color #999
        .shop_header_title
          color #999
          font-size 14px
          line-height 20px
</style>
