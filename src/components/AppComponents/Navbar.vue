<template xmlns:el-col="http://www.w3.org/1999/xhtml">
  <div>
    <el-menu theme="dark" :default-active="activeIndex"
             class="el-menu-demo" mode="horizontal" @select="handleSelect">
      <el-menu-item index="1" @click.native="changePage('resize')">图片裁剪</el-menu-item>
      <el-menu-item index="2" @click.native="changePage('deNoise')">图片降噪</el-menu-item>
      <el-menu-item index="3" @click.native="changePage('addNoise')">图片加噪</el-menu-item>
      <el-menu-item index="4" @click.native="changePage('imageWaterMark')">文字水印</el-menu-item>
      <el-menu-item index="5" @click.native="changePage('changestyle')">图像风格转换</el-menu-item>
      <el-menu-item index="6" @click.native="changePage('passport')">证件照换底</el-menu-item>
    </el-menu>
    <div :is="currentView"></div>
  </div>
</template>

<script>
import Home from '@/components/Home'
import resize from '@/components/page/resize'
import passport from '@/components/page/passport'
import imageWaterMark from '@/components/page/imageWaterMark'
import addNoise from '../page/addNoise'
import deNoise from '../page/deNoise'
import changestyle from '../page/changestyle'

export default {
  components: {
    Home,
    resize,
    deNoise,
    addNoise,
    imageWaterMark,
    changestyle,
    passport
  },
  data () {
    return {
      currentView: 'Home'
    }
  },
  methods: {
    handleSelect (key, keyPath) {
      console.log(key, keyPath)
    },
    changePage (pageItem) {
      this.currentView = pageItem
      console.log(this.currentView + '--------------------------------------------------')
    }
  }
}
</script>

<style>
.el-menu {
  background-color: #483c42;
  color: var(--el-text-color-primary);
  text-align: center;
  line-height: 60px;
}
</style>
