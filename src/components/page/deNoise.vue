<template>
  <div>
    <div>图片降噪</div>
    <div id="left_part">
      <image-upload @onUploadImg="changeBase64"></image-upload>
      <div id="change_btn">
        <el-button type="primary" @click="submit()">确认</el-button>
      </div>
    </div>
    <div id="right_part" v-loading="isLoading">
      <image-display :displayUrl="displayUrl"></image-display>
    </div>
  </div>
</template>
<script>
import ImageUpload from '../AppComponents/imageUpload'
import imageDisplay from "../AppComponents/imageDisplay";
import {serverUrl} from "../../utils/global";
import request from '../../utils/request'

export default {
  name: 'deNoise',
  components: {
    ImageUpload,
    imageDisplay
  },
  data: function () {
    return {
      imagebase64: '',
      displayUrl: '',
      isLoading:false,
    }
  },
  methods: {
    changeBase64(base64) {
      this.imagebase64 = base64
    },
    submit() {
      const that = this
      this.isLoading=true
      console.log('开始加载',this.isLoading)
      request.post(serverUrl, {
        type: 'denoise',
        img: this.imagebase64,
        name: 'person.jpg',
      }).then(res => {
        console.log(res)
        that.displayUrl = res.message
        that.isLoading=false
        console.log('完成',that.isLoading)
        this.$message({
          type: "success",
          message: "提交成功"
        })
        // this.displayUrl = res.data
      })
    }
  }
}
</script>

<style scoped>
#left_part {
  background-color: #eee;
  float: left;
  width: 50%;
  height: calc(100vh - 180px);
}
#right_part {
  background-color: #eee;
  float: right;
  width: 49%;
  height: calc(100vh - 180px);
}
#change_btn {
  visibility: hidden;
}
</style>
