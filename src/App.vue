<template>
  <div class="common-layout">
    <el-container>
      <el-header>
        <div>
          <mynavbar></mynavbar>
        </div>
      </el-header>
    </el-container>
  </div>
</template>

<script>
import mynavbar from '@/components/AppComponents/Navbar'

export default {
  name: 'App',
  components: {
    mynavbar
  }
}
</script>

<style >
.el-header {

  text-align: center;
  line-height: 60px;
  padding: 10px;
}


</style>
