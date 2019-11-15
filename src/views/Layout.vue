<template>
  <div class="container">
    <van-nav-bar
      fixed title="黑马头条" right-text="搜索"
      @click-right="$router.push('/search')"/>
    <div class="my-wrapper">
      <router-view></router-view>
    </div>
    <van-tabbar route>
      <van-tabbar-item to="/" icon="home-o">首页</van-tabbar-item>
      <van-tabbar-item to="/question" icon="chat-o">问答</van-tabbar-item>
      <van-tabbar-item to="/video" icon="video-o">视频</van-tabbar-item>
      <van-tabbar-item to="/user" icon="user-o">我的</van-tabbar-item>
    </van-tabbar>
  </div>
</template>

<script>
import Vue from 'vue'
import { NavBar, Tabbar, TabbarItem } from 'vant'

Vue.use(NavBar)
Vue.use(Tabbar).use(TabbarItem)

export default {
  data () {
    return {

    }
  },
  methods: {
  }
}
</script>

<style scoped lang='less'>
.container{
    width: 100%;
    height: 100%;
    position: relative;
    .my-wrapper{
      width: 100%;
      height: 100%;
      overflow: hidden;
      padding-top: 46px;
      padding-bottom: 50px;
      box-sizing: border-box;
      &.noTop{
        padding-top: 0;
      }
    }
  }

</style>
