<template>
  <view>
    <text>{{text}}</text>
    <button type="primary" @click="handleClick">新增数据</button>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        text: ''
      }
    },
    onLoad() {
      this.getOpenerEventChannel().on('acceptDataFromOpenerPage', (data) => {
        this.text = data.item.name
      })
    },
    methods: {
      handleClick() {
        this.getOpenerEventChannel().emit("refresh")
      }
    },
  }
</script>

<style lang="scss" scoped>
</style>