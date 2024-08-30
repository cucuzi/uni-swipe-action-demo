<template>
  <view>
<!--    <button type="primary" @click="handleAdd">新增数据</button>-->
    <uni-swipe-action ref="swipeAction">
      <uni-swipe-action-item v-for="item in list" :key="item.id" :right-options="rightOptions"
        @click="handleDelete(item)">
        <view class="item-box" @click="handleClick(item)">
          <view>
            <text>{{ item.name }}</text>
          </view>
          <view class="item-box-subtitle">
            <text>选取日期：{{ item.date }}</text>
          </view>
        </view>
      </uni-swipe-action-item>
    </uni-swipe-action>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        rightOptions: [{
          text: '删除',
          style: {
            backgroundColor: 'rgb(255,58,49)'
          }
        }],
        list: [],
      }
    },
    onLoad() {
      for (let i = 0; i < 3; i++) {
        this.list.push({
          id: i,
          name: `测试数据${i}`,
          date: "2024-06-05"
        })
      }
    },
    onShow() {
      this.$nextTick(() => {
        this.$refs.swipeAction.resize()
      })
    },
    methods: {
      handleQuery(pageNum, isLoading) {},
      handleDelete(obj) {
        this.list.splice(this.list.findIndex(item => item.id === obj.id), 1)
      },
      handleClick(item) {
        uni.navigateTo({
          url: `/pages/demo/detail`,
          events: {
            refresh: () => {
              this.handleAdd()
            },
          },
          success: (res) => {
            res.eventChannel.emit('acceptDataFromOpenerPage', {
              item
            })
          }
        })
      },
      handleAdd() {
        let i = this.list.length
        let max = this.list.length + 3
        for (; i < max; i++) {
          this.list.push({
            id: i,
            name: `测试数据${i}`,
            date: "2024-06-05"
          })
        }
      }
    },
  }
</script>

<style lang="scss" scoped>
  .item-box {
    width: 100%;
    padding: 6px 4px;
    border-bottom: 1px solid rgba(0, 0, 0, .3);
    box-sizing: border-box;

    .item-box-subtitle {
      color: $uni-info;
    }
  }
</style>