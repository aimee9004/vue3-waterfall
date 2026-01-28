<template>
  <div
    class="m-waterfall-wrap"
    :style="`background-color: ${backgroundColor}; width: ${totalWidth}px; height: ${height}px;`"
  >
    <img
      class="u-img"
      v-for="(item, index) in imagesProperty"
      :key="index"
      :style="`width: ${imageWidth}px; top: ${item?.top}px; left: ${item?.left}px;`"
      :src="imageData[index].imgUrl"
      :title="imageData[index].title"
      :alt="imageData[index].title"
    />
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from "vue";

// 定义组件名称
defineOptions({
  name: "Waterfall",
});

// 定义props
const props = defineProps({
  imageData: {
    type: Array,
    required: true,
    default: () => [],
  },
  columnCount: {
    type: Number,
    default: 3,
  },
  columnGap: {
    type: Number,
    default: 30,
  },
  totalWidth: {
    type: Number,
    default: 1200,
  },
  backgroundColor: {
    type: String,
    default: "#F2F4F8",
  },
});

// 定义响应式数据
const imagesProperty = ref([]);
const preImages = ref(new Array(props.columnCount).fill(0));

// 计算属性
const imageWidth = computed(() => {
  return (props.totalWidth - 4 * props.columnGap) / props.columnCount;
});

const height = computed(() => {
  return Math.max(...preImages.value) + props.columnGap;
});

// 监听imageData变化
watch(
  () => props.imageData,
  (to) => {
    onPreload();
    imagesProperty.value.splice(to.length);
  },
  { deep: true },
);

// 组件挂载后执行
onMounted(() => {
  if (props.imageData.length) {
    onPreload();
    imagesProperty.value.splice(props.imageData.length);
  }
});

// 方法定义
const getPosition = (i, height) => {
  if (i < props.columnCount) {
    preImages.value[i] = props.columnGap + height;
    return {
      top: props.columnGap,
      left: (imageWidth.value + props.columnGap) * i + props.columnGap,
    };
  } else {
    const top = Math.min(...preImages.value);
    const index = preImages.value.indexOf(top);
    preImages.value[index] = top + props.columnGap + height;
    return {
      top: top + props.columnGap,
      left: (imageWidth.value + props.columnGap) * index + props.columnGap,
    };
  }
};

const onLoad = (url, i) => {
  return new Promise((resolve) => {
    const image = new Image();
    image.src = url;
    image.onload = function () {
      const imageHeight = image.height / (image.width / imageWidth.value);
      imagesProperty.value[i] = {
        width: imageWidth.value,
        height: imageHeight,
        ...getPosition(i, imageHeight),
      };
      resolve("load");
    };
  });
};

const onPreload = async () => {
  const len = props.imageData.length;
  for (let i = 0; i < len; i++) {
    await onLoad(props.imageData[i].imgUrl, i);
  }
};
</script>
<style lang="less" scoped>
.m-waterfall-wrap {
  position: relative;
  margin: 0 auto;
  .u-img {
    position: absolute;
    display: inline-block;
    object-fit: contain;
    vertical-align: bottom;
    transition: all 0.3s ease;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    &:hover {
      transform: translateY(-5px);
      box-shadow: 0 5px 20px rgba(0, 0, 0, 0.15);
    }
  }
}
</style>
