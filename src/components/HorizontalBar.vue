<template>
  <div>
    <div>【大区数据信息】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import * as echarts from "echarts";
const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});
console.log(props.data);

// 1.初始化echarts实例
let myChart = null;
const target = ref(null);
onMounted(() => {
  myChart = echarts.init(target.value);
  // 渲染ecahrts
  renderChart();
});

// 2.构建option 配置对象
const renderChart = () => {
  const options = {
    // X轴展示数据，
    xAxis: {
      show: false,
      type: "value",
      max: function (value) {
        return parseInt(value.max * 1.2);
      },
    },
    // y轴展示数据
    yAxis: {
      type: "category",
      // 根据根据服务端数据筛选
      data: props.data.regions.map((item) => item.name),
      // 反向展示
      inverse: true,
      // 不展示轴线
      axisLine: {
        show: false,
      },
      // 不展示刻度
      axisTick: {
        show: false, // 取消 Y 轴刻度
      },
      // 文字色值
      axisLabel: {
        color: "#9EB1C8",
      },
    },
    // 图表绘制的位置,对应上下左右
    grid: {
      top: 0,
      left: 0,
      bottom: 0,
      right: 0,
      containLabel: true,
    },
    // 核心配置
    series: [
      {
        // 图表类型
        type: "bar",
        // 数据筛选
        data: props.data.regions.map((item) => ({
          name: item.name,
          value: item.value,
        })),
        // 显示背景
        showBackground: true,
        // 背景色
        backgroundStyle: {
          color: "rgba(180, 180, 180, 0.2)",
        },
        // 每个轴的样式
        itemStyle: {
          color: "#479AD3", // 设置柱子的颜色
          barBorderRadius: 5, // 设置柱子的圆角
          shadowColor: "rgba(0, 0, 0, 0.3)", // 设置柱子的阴影颜色
          shadowBlur: 5, // 设置柱子的阴影模糊大小
        },
        // 轴宽度
        barWidth: 12,
        // 轴上的字体
        label: {
          show: true,
          // 设置标签位置为右侧
          position: "right",
          textStyle: {
            // 设置标签文本颜色
            color: "#fff",
          },
        },
      },
    ],
  };
  // 3.通过 实例.setOption(options)
  myChart.setOption(options);
};

watch(
  () => props.data,
  () => {
    renderChart();
  }
);
</script>

<style lang="scss" scoped>
</style>