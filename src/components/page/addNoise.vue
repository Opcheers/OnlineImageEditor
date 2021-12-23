<template>
  <div>
    <div>图片加噪</div>
    <div id="left_part">
      <image-upload @onUploadImg="changeBase64"></image-upload>
      <el-button id="change_btn" type="primary" @click="submit()">
        确认
      </el-button>
    </div>
    <div id="right_part" v-loading="isLoading">
      <image-display :displayUrl="displayUrl"></image-display>
    </div>
  </div>
</template>
<script>
import ImageUpload from '../AppComponents/imageUpload';
import ImageDisplay from "../AppComponents/imageDisplay";
import {serverUrl} from "../../utils/global";
import request from '../../utils/request';

export default {
  name: 'addNoise',
  components: {
    ImageDisplay,
    ImageUpload
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
      request.post(serverUrl, {
        type: 'addNoise',
        img: this.imagebase64,
        name: 'fengjing.jpg',
      }).then(res => {
        console.log(res)
        that.displayUrl = res.message
        that.isLoading=false
        this.$message({
          type: "success",
          message: "提交成功"
        })
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
  vertical-align: center;

}
#change_btn {
  visibility: hidden;
}
</style>
