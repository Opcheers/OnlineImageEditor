<template>
  <div class="image_display">
    <img class="bigImg" :src=displayUrl v-if="displayUrl">
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
  margin-top: 100px;
}
.bigImg {
  display: flex;
  width: 200px;
  height: 200px;
  margin-left: auto;
  margin-right: auto;
  vertical-align: center;
}
</style>
