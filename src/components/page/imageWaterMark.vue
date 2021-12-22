<template>
  <div>
    <div>文字水印</div>
    <div id="left_part">
      <image-upload @onUploadImg="changeBase64"></image-upload>
      <div id="change_btn" class=" grid-content bg-purple">
        <el-row type="flex" class="row-bg" justify="center">
          水印&nbsp&nbsp
          <el-input v-model="demandInfo.watermark_words" placeholder="水印文字" style="width: 50%"></el-input>
        </el-row>
        <el-row type="flex" class="row-bg" justify="center">
          <div class="name">位置&nbsp&nbsp</div>
          <el-select v-model="demandInfo.position" placeholder="请选择水印添加位置" style="width: 50%">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-row>
        <el-button type="primary" @click="addDemand">提交</el-button>
      </div>
    </div>
    <div id="right_part">
      <image-display :displayUrl="displayUrl"></image-display>
    </div>
  </div>
</template>

<script>
import ImageUpload from '../AppComponents/imageUpload'
import imageDisplay from "../AppComponents/imageDisplay";
import {serverUrl} from "../../utils/global";
import request from '../../utils/request'

let inputElement = null
export default {
  name: 'imageWaterMark',
  components: {
    ImageUpload,
    imageDisplay
  },
  data() {
    return {
      valueUrl: '',
      options: [{
        value: 'center',
        label: '中间'
      }, {
        value: 'lefttop',
        label: '左上角'
      }, {
        value: 'righttop',
        label: '右上角'
      }, {
        value: 'leftbottom',
        label: '左下角'
      }, {
        value: 'rightbottom',
        label: '右下角'
      }],
      value: '',
      demandInfo: {
        watermark_words: '',
        position: ''
      },
      imagebase64: '',
      displayUrl:'',
    }
  },
  methods: {
    changeBase64(base64){
      this.imagebase64=base64
    },
    addDemand() {
      let that = this
      if (!that.demandInfo.watermark_words) {
        that.$message.error("请输入水印文字")
        return
      }
      if (!that.demandInfo.position) {
        that.$message.error("请选择水印位置")
        return
      }
      let map = {
        watermark_words: that.demandInfo.watermark_words,
        position: that.demandInfo.position
      }
      console.log(map)
      request.post(serverUrl,{
        personal:"{\"text\":\""+that.demandInfo.watermark_words+"\",\"loca\":\""+that.demandInfo.position+"\"}",
        type:'watermark',
        img:this.imagebase64,
        name:'shuiyin.jpg',
      } ).then(res => {
        console.log(res)
        that.displayUrl=res.message
        this.$message({
          type: "success",
          message: "提交成功"
        }).catch(function (e) {
          console.log(e)
        })
      })
    }
  },
}

</script>

<style>
.bigImg {
  display: flex;
  width: 200px;
  height: 200px;
  margin-left: auto;
  margin-right: auto;
}
#left_part {
  background-color: #eee;
  float: left;
  width: 50%;
  height: 440px;
}
#right_part {
  background-color: #eee;
  float: right;
  width: 49%;
  height: 440px;
}
#change_btn {
  visibility: hidden;
}
</style>
