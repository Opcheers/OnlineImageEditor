<template>

  <div class="image_upload">
    <el-button type="primary" @click="toGetImg">
      <span style="vertical-align: middle;">选择要修改的图片</span>
    </el-button>
    <img class="bigImg" :src=valueUrl v-if="valueUrl">
  </div>

</template>

<script>

let inputElement = null
export default {
  data() {
    return {
      valueUrl: '',
      imgBase64: ''
    }
  },
  methods: {
    toGetImg () {
      if (inputElement === null) {
        // 生成文件上传的控件
        inputElement = document.createElement('input')
        inputElement.setAttribute('type', 'file')
        inputElement.style.display = 'none'

        if (window.addEventListener) {
          inputElement.addEventListener('change', this.uploadFile, false)
        } else {
          inputElement.attachEvent('onchange', this.uploadFile)
        }

        document.body.appendChild(inputElement)
      }
      inputElement.click()
    },
    uploadFile (el) {
      if (el && el.target && el.target.files && el.target.files.length > 0) {
        console.log(el)
        const files = el.target.files[0]
        const isLt2M = files.size / 1024 / 1024 < 2
        const size = files.size / 1024 / 1024
        console.log('图片大小:'+size)
        // 判断上传文件的大小
        if (!isLt2M) {
          this.$message.error('上传图片大小不能超过 2MB!')
        } else if (files.type.indexOf('image') === -1) { // 如果不是图片格式
          // this.$dialog.toast({ mes: '请选择图片文件' });
          this.$message.error('请选择图片文件')
        } else {
          const that = this
          const reader = new FileReader() // 创建读取文件对象
          reader.readAsDataURL(el.target.files[0]) // 发起异步请求，读取文件
          reader.onload = function (res) { // 文件读取完成后
            // 读取完成后，将结果赋值给img的src
            console.log('文件读取结果',res)
            that.valueUrl = this.result
            this.imgBase64 = this.result
            // console.log('图片地址：' + this.result)
            // console.log('图片base64：' + this.imgBase64)
            that.$emit('onUploadImg',this.imgBase64)
            // 数据传到后台
            // const formData = new FormData()
            // formData.append('file', files); // 可以传到后台的数据
            document.getElementById('change_btn').style.visibility = 'visible'
          }
        }
      }
    }
  },
  beforeDestroy () {
    if (inputElement) {
      if (window.addEventListener) {
        inputElement.removeEventListener('change', this.onGetLocalFile, false)
      } else {
        inputElement.detachEvent('onchange', this.onGetLocalFile)
      }
      document.body.removeChild(inputElement)
      inputElement = null
      console.log('========inputelement destroy')
    }
  }
}
</script>

<style scoped>

.bigImg {
  display: flex;
  max-width: 500px;
  max-height: 300px;
  margin-left: auto;
  margin-right: auto;
  vertical-align: center;
}
</style>
