<template>
  <div class="image_display">
    <img class="bigImg" :src=displayUrl v-if="displayUrl">
    <a class="downloadBtn" v-if="displayUrl" :href="displayUrl">下载图片</a>
  </div>
</template>

<script>
export default {
  name: 'imageDisplay',
  props:{
    displayUrl:''
  }
}
</script>

<style scoped>

.image_display{
  margin-top: 50px;
}
.bigImg {
  display: flex;
  max-width: 500px;
  max-height: 300px;
  margin-left: auto;
  margin-right: auto;
  vertical-align: center;
}
.downloadBtn{
  text-decoration: none;
  padding: 10px 20px;
  background: #409eff;
  color:white;
  box-shadow: 1px 1px 2px #409eff;
  font-size: 14px;
  border-radius: 2px;
}
.downloadBtn:hover{
  opacity: 0.8;
}
</style>
