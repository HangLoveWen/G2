<template>
  <div id="linechart"></div>
</template>
<script>
import { onMounted } from "vue";
import * as G2 from "@antv/g2";
import { vertical } from "@antv/matrix-util/lib/ext";
export default {
  setup() {
    onMounted(() => {
      const data = [
        { year: "2022-10-13", value: 0 },
        { year: "2022-10-14", value: 0 },
        { year: "2022-10-15", value: 0 },
        { year: "2022-10-16", value: 10 },
        { year: "2022-10-17", value: 0 },
      ];
      // Step 1: 创建 Chart 对象
      const chart = new G2.Chart({
        container: "linechart", // 指定图表容器 ID
        width: 600, // 指定图表宽度
        height: 300, // 指定图表高度
        padding: 50,
      });
      // Step 2: 载入数据源
      chart.data(data);

      // Step 3：创建图形语法，绘制折线图
      chart.scale({
        year: {
          range: [0, 1],
        },
        value: {
          min: 0,
          max: 100,
          nice: true,
        },
      });
      chart.tooltip({
        showCrosshairs: true, // 展示 Tooltip 辅助线
        shared: true,
      });
      chart.axis("year", {
        grid: {
          line: vertical,
          tickLine: {
            width: 20,
          },
        },
      });
      chart.axis("value", {
        tickLine: {},
      });
      chart.legend({
        custom: true,
        position: "right-top",
        offsetY: 20,
        offsetX: 10,
        flipPage: false,
        items: [
          {
            name: "空值数量",
            position: "bottom",
            marker: {
              symbol: "square",
              style: {
                fill: "#93b1ee",
              },
              clickable: false,
            },
          },
        ],
      });
      chart.line().position("year*value").label("value");
      chart.point().position("year*value");

      // Step 4: 渲染图表
      chart.render();
    });
  },
};
</script>
<style></style>
