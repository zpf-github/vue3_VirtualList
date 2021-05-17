<template>
  <div class="home">
    <VirtualList
      :data="data"
      :viewHeight="viewHeight"
      :itemHeight="itemHeight"
    ></VirtualList>
  </div>
</template>

<script>
import VirtualList from "@/components/VirtualList.vue";

export default {
  name: "Home",
  components: {
    VirtualList,
  },
  setup() {
    // 做一个1~10000的数组
    const data = Array.from(Array(10000), (v, k) => k);
    // 列表的高
    const viewHeight = 700;
    // 列表item的高
    const itemHeight = 50;
    return {
      data,
      viewHeight,
      itemHeight,
    };
  },
};
</script>
