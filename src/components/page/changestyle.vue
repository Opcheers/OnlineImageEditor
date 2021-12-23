<template>
  <div>
    <div>风格转换</div>
    <div id="left_part">
      <image-upload @onUploadImg="changeBase64"></image-upload>
      <div id="change_btn">
        <template>
          <el-row>
            <span>图片风格&nbsp&nbsp</span>
            <el-select
              v-model="imagestyle"
              placeholder="请选择图片风格"
              @change="displaystyle"
              style="width: auto"
            >
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
            <el-button type="primary" @click="addDemand">确认</el-button>
          </el-row>
        </template>
        <img id="styleImg" class="bigImg" :src="styleUrl" v-if="styleUrl" />
      </div>
    </div>
    <div id="right_part" v-loading="isLoading">
      <image-display :displayUrl="displayUrl"></image-display>
    </div>
  </div>
</template>

<script>
import ImageUpload from '../AppComponents/imageUpload'
import ImageDisplay from "../AppComponents/imageDisplay";
import {serverUrl} from "../../utils/global";
import request from '../../utils/request';

let inputElement = null
export default {
  name: 'passport',
  components: {
    ImageUpload,
    ImageDisplay
  },
  data() {
    return {
    isLoading:false,
      options: [{
        value: '1',
        label: '1'
      }, {
        value: '2',
        label: '2'
      }, {
        value: '3',
        label: '3'
      }, {
        value: '4',
        label: '4'
      }, {
        value: '5',
        label: '5'
      }, {
        value: '6',
        label: '6'
      }, {
        value: '7',
        label: '7'
      }, {
        value: '8',
        label: '8'
      }, {
        value: '9',
        label: '9'
      }, {
        value: '10',
        label: '10'
      }],
      value: '',
      imagestyle: '',
      imagebase64: '',
      displayUrl: '',
      styleUrl: ''
    }
  },
  methods: {
    changeBase64(base64) {
      this.imagebase64 = base64
    },
    displaystyle() {
      let that = this
      that.styleUrl = '../static/imagestyle/' + that.imagestyle + '.jpg'
      console.log("change事件：" + that.styleUrl)
    },
    addDemand() {
      let that = this
      if (!that.imagestyle) {
        that.$message.error("请选择图片风格")
        return
      }
      this.isLoading=true
      request.post(serverUrl, {
        personal: "{\"style\":\"" + that.imagestyle + ".jpg\"}",
        type: 'style',
        img: this.imagebase64,
        name: 'cat2.jpg',
      }, {
        timeout: 60000
      }).then(res => {
        console.log(res)
        this.isLoading=false
        that.displayUrl = res.message
        this.$message({
          type: "success",
          message: "提交成功"
        })
      }).catch(function (e) {
        console.log(e)
      })
    }
  },
  beforeDestroy() {
    if (inputElement) {
      if (window.addEventListener) {
        inputElement.removeEventListener('change', this.onGetLocalFile, false)
      } else {
        inputElement.detachEvent('onchange', this.onGetLocalFile)
      }
      document.body.removeChild(inputElement)
      inputElement = null
      console.log('========changestyle inputelement destroy')
    }
  }
}
</script>

<style scoped>
#left_part {
  background-color: #eee;
  float: left;
  width: 50%;
  height: 740px;
}
#right_part {
  background-color: #eee;
  float: right;
  width: 49%;
  height: 740px;
}
.bigImg {
  display: flex;
  max-width: 500px;
  max-height: 300px;
  margin-left: auto;
  margin-right: auto;
  vertical-align: center;
}
#change_btn {
  visibility: hidden;
}
</style>
