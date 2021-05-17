<template>
  <div
    class="item_box"
    :style="`height:${viewHeight}px`"
    @scroll="handleScroll"
  >
    <div :style="`transform: translateY(${state.offSetY}px)`">
      <div
        class="item"
        :style="`height:${itemHeight}px;line-height: ${itemHeight}px`"
        v-for="item in list"
        :key="item"
      >
        {{ "虚拟列表" + item }}
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive } from "vue";
export default {
  name: "VirtualList",
  props: {
    data: Array, // 源数据
    viewHeight: Number, // 列表高度
    itemHeight: Number, // 列表item高度
  },

  setup(props) {
    const state = reactive({
      offSetY: 0, // 偏移量
      lastTime: new Date().getTime(), // 上一次操作后的时间戳，节流时使用
    });

    // 每页几个item，多加4个为避免出错
    const showNum = props.viewHeight / props.itemHeight + 4;
    // 列表的初始数据
    const list = ref(props.data.slice(0, showNum));

    // 滚动事件函数
    const handleScroll = (e) => {
      if (new Date().getTime() - state.lastTime > 20) {
        // 计算列表偏移量
        state.offSetY = e.target.scrollTop - (e.target.scrollTop % showNum);
        // 动态修改列表数据
        list.value = props.data.slice(
          Math.floor(e.target.scrollTop / props.itemHeight),
          Math.floor(e.target.scrollTop / props.itemHeight) + showNum
        );
        state.lastTime = new Date().getTime();
      }
    };
    return {
      state,
      list,
      handleScroll,
    };
  },
};
</script>

<style scoped lang="less">
.item_box {
  overflow: scroll;
  .item {
    text-align: center;
    &:nth-child(2n) {
      background-color: #cccccc;
    }
    &:nth-child(2n + 1) {
      background-color: #f8f8f8;
    }
  }
}
</style>
