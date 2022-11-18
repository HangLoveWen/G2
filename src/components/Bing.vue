<template>
  <div id="linechart"></div>
</template>
<script>
import DataSet from "@antv/data-set";
import { onMounted } from "vue";
import * as G2 from "@antv/g2";
export default {
  name: "Bing",
  setup() {
    onMounted(() => {
      const data = [
        { name: " 应用资产1", type: "Behavior", value: 204 },
        { name: " 应用资产2", type: "Behavior", value: 302 },
        { name: "应用资产3", type: "Behavior", value: 300 },
        { name: "应用资产4", type: "Identifier", value: 204 },
        { name: "应用资产5", type: "Identifier", value: 302 },
        { name: "应用资产6", type: "Identifier", value: 300 },
        { name: "应用资产7", type: "Personality", value: 900 },
      ];
      const ds = new DataSet();
      const dv = ds.createView();
      dv.source(data).transform({
        type: "percent",
        field: "value",
        dimension: "type",
        as: "percent",
      });

      const colorMap = {
        Behavior: "#1b69d5",
        Personality: "#6096e1",
        Identifier: "#c6e0ff",
      };
      const chart = new G2.Chart({
        container: "linechart",
        autoFit: true,
        height: 500,
      });
      chart.legend({
        position: "top",
        offsetX: 400,
        offsetY: 150,
        itemWrap: true,
      });
      const outterView1 = chart.createView();
      outterView1.data(dv.rows);
      outterView1.legend(false);
      outterView1.coordinate("theta", {
        radius: 0.5,
        innerRadius: 0.5,
      });
      outterView1.tooltip(false);
      outterView1
        .interval()
        .adjust("stack")
        .position("percent")
        .tooltip("name*type*value", (name, type, value) => {
          return {
            name: name,
            type: type,
            value: '<span style="color: #1890ff;">' + value + "</span>",
          };
        })
        .color("type", (val) => colorMap[val])
        .style({
          stroke: "white",
          lineWidth: 3,
        })
        .label(false);
      const ds2 = new DataSet();
      const dv2 = ds2.createView();
      dv2.source(data).transform({
        type: "percent",
        field: "value",
        dimension: "name",
        as: "percent",
      });
      const outterView = chart.createView();
      outterView.data(dv2.rows);
      outterView.coordinate("theta", {
        innerRadius: 0.5 / 0.8,
        radius: 0.8,
      });
      chart.tooltip({
        position: "bottom",
        showCrosshairs: false,
        showMarkers: false,
        showTitle: false,
        itemTpl: `
    <li class="g2-tooltip-list-item" v-for="(item,index) in data">
       <span class="g2-tooltip-name">{type}</span>
       <hr style="margin-top: 10px;">
       <span class="g2-tooltip-name" style="display: inline-block;margin-top: 10px;">{name}</span>
        <span class="g2-tooltip-value" style="display: inline-block;margin-top: 10px;font-size:5px">{value}</span>
        <br/>
        <span class="g2-tooltip-name" style="display: inline-block;margin-top: 10px;">{name}</span>
        <span class="g2-tooltip-value" style="display: inline-block;margin-top: 10px;font-size:5px">{value}</span>
        <br/>
        <span class="g2-tooltip-name" style="display: inline-block;margin-top: 10px;">{name}</span>
        <span class="g2-tooltip-value" style="display: inline-block;margin-top: 10px;font-size:5px">{value}</span>
           `,
      });
      outterView
        .interval()
        .adjust("stack")
        .position("percent")
        .color("type*name", (type, name) => colorMap[type])
        .tooltip("name*type*value", (name, type, value) => {
          return {
            name: name,
            type: type,
            value: '<span style="color: #1890ff;">' + value + "</span>",
          };
        })
        .style({
          stroke: "white",
          lineWidth: 3,
        })
        .label(false);
      chart.interaction("element-active");
      chart.interaction("element-selected");
      chart.render();
    });
  },
};
</script>
<style></style>
