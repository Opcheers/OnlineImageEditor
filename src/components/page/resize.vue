<template>
  <div class="image_cut">
    <div>图片裁剪</div>
    <div id="left_part">
      <image-upload @onUploadImg="changeBase64"></image-upload>
      <div id="change_btn">
        <template>
          <el-row>
            宽度：
            <el-input v-model="width" placeholder="请输入宽度"/>
            高度：
            <el-input v-model="height" placeholder="请输入高度"/>
            <el-button @click="changeSize()">确认</el-button>
          </el-row>
        </template>
      </div>
    </div>
    <div id="right_part" v-loading="isLoading" >
      <image-display :displayUrl="displayUrl"></image-display>
    </div>
  </div>
</template>

<script>
import ImageUpload from '../AppComponents/imageUpload'
import ImageDisplay from "../AppComponents/imageDisplay";
import {serverUrl} from "../../utils/global";
import request from '../../utils/request';

export default {
  name: 'resize',
  data() {
    return {
      width: '',
      height: '',
      imagebase64: '',
      displayUrl: '',
      isLoading:false,
    }
  },
  components: {
    ImageUpload,
    ImageDisplay
  },
  methods: {
    changeBase64(base64) {
      this.imagebase64 = base64
    },
    changeSize() {
      console.log('width:' + this.width + ', height:' + this.height)
      const that = this
      this.isLoading=true
      request.post(serverUrl, {
        personal: '{\"width\":\"' + that.width + '\",' + '\"height\":\"' + that.height + '\"}',
        type: 'resize',
        img: this.imagebase64,
        name: 'cat2.jpg',
      }).then(res => {
        console.log(res)
        that.isLoading=false
        that.displayUrl = res.message
        this.$message({
          type: "success",
          message: "提交成功"
        })
      }).catch(function (e) {
        this.isLoading=false
        console.log(e)
      })
    }
  }
}

</script>

<style scoped>
.el-input {
  width: auto;
}
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
