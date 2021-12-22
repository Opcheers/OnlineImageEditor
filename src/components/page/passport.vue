<template>
  <div>
    <div>证件照换底</div>
    <div id="left_part">
      <image-upload @onUploadImg="changeBase64"></image-upload>
      <div id="change_btn">
        <el-row type="flex" class="row-bg" justify="center">
          <div class="name">底色&nbsp&nbsp</div>
          <el-select v-model="bkgcolor" placeholder="请选择底色" style="width: 50%">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-row>
        <el-button type="primary" @click="addDemand">确认</el-button>
      </div>
    </div>
    <div id="right_part">
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
  name: 'passport',
  components: {
    ImageUpload,
    ImageDisplay
  },
  data() {
    return {
      options: [{
        value: 'red',
        label: '红色'
      }, {
        value: 'blue',
        label: '蓝色'
      }, {
        value: 'white',
        label: '白色'
      }],
      value: '',
      bkgcolor: '',
      imagebase64: '',
      displayUrl: '',
    }
  },
  methods: {
    changeBase64(base64) {
      this.imagebase64 = base64
    },
    addDemand() {
      let that = this
      if (!that.bkgcolor) {
        that.$message.error("请选择底色！")
        return
      }
      let map = {
        bkgcolor: that.bkgcolor
      }
      console.log(map)

      request.post(serverUrl, {
        personal: "{\"color\":\"" + that.bkgcolor + "\"}",
        type: 'passport',
        img: this.imagebase64,
        name: 'huandi.jpg',
      }).then(res => {
        console.log(res)
        that.displayUrl = res.message
        this.$message({
          type: "success",
          message: "提交成功"
        }).catch(function (e) {
          console.log(e)
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
