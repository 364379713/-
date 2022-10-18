<template>
  <div id="odiv"></div>
</template>

<script>
let d3 = require("./d3.v5.js");
export default {
  data() {
    return {};
  },
  computed: {},
  mounted() {
    console.log(11);
    this.init();
  },
  methods: {
    init() {
      let echarts = this.$echarts;
      var chartDom = document.getElementById("odiv");
      var myChart = echarts.init(chartDom, "walden");
      var data = [
        {
          name: "1",
          draggable: true,
          //   x: 0,
          //   y: 100,
          draggable: true,
          fixed: true,
        },
        {
          name: "2",
          draggable: true,
          //   x: 100,
          //   y: 100,
          draggable: true,
          fixed: true,
        },
        {
          name: "3",
          //   x: 200,
          //   y: 100,
          draggable: true,
          fixed: true,
        },
        {
          name: "4",
          //   x: 300,
          //   y: 0,
          draggable: true,
          fixed: true,
        },
        {
          name: "5",
          //   x: 300,
          //   y: 100,
          draggable: true,
          fixed: true,
        },
        {
          name: "6",
          draggable: true,
          //   x: 300,
          //   y: 200,
          draggable: true,
          fixed: true,
        },
        {
          name: "7",
          draggable: true,
          //   x: 300,
          //   y: 200,
          draggable: true,
          fixed: true,
        },
        {
          name: "11",
          draggable: true,
          //   x: 300,
          //   y: 200,
          draggable: true,
          fixed: true,
        },
        {
          name: "12",
          //   x: 300,
          //   y: 200,
          draggable: true,
          fixed: true,
        },
        {
          name: "8",
          draggable: true,
          //   x: 300,
          //   y: 200,
          draggable: true,
          fixed: true,
        },
        {
          name: "9",
          draggable: true,
          //   x: 300,
          //   y: 200,
          category: 1,
          draggable: true,
          fixed: true,
        },
        {
          name: "10",
          draggable: true,
          //   x: 300,
          //   y: 200,
          category: 1,
          draggable: true,
          fixed: true,
        },
      ];
      //   data
      d3.forceSimulation(data);
      console.log("****", data);
      const series = {
        type: "graph",
        roam: true,
        nodeScaleRatio: 0,
        layout: "force", // 如果想要使用graph的拖拽功能，layout必须是force
        autoCurveness: false,
        scaleLimit: {
          min: 0.5,
          max: 3,
        },
        symbolSize: 50,
        label: {
          show: true,
          align: "center",
          textStyle: { fontSize: 12, color: "#fff" },
          borderRadius: [0, 0, 5, 5],
        },
        labelLayout: function (param) {
          return {
            moveOverlap: true,
            hideOverlap: true,
          };
        },
        edgeSymbol: ["none", "arrow"],
        edgeSymbolSize: [4, 8],
        data: data,
        links: [
          { source: 6, target: 7, value: "" },
          { source: 1, target: 2, value: "" },
          { source: 2, target: 3, value: "" },
          { source: 2, target: 4, value: "" },
          { source: 2, target: 5, value: "" },
        ],
        lineStyle: {
          opacity: 0.9,
          width: 2,
          color: "#333",
        },
      };
      let option = { series };
      myChart.setOption(option);
      setTimeout(function () {
        // 源码中在dragend里将node设为了unfixed，变成了浮动
        // 因此需要增加一下dragend监听，将node设置成固定
        var seriesModel = myChart.getModel().getSeriesByIndex(0);
        seriesModel.getData().graph.eachNode((node) => {
          var el = node.getGraphicEl();
          var idx = node.dataIndex;
          var itemModel = node.getModel();
          var draggable = itemModel.get("draggable");
          if (draggable) {
            el.on("dragend", function (el) {
              var forceLayout = seriesModel.forceLayout;
              if (forceLayout) {
                forceLayout.setFixed(idx);
              }
            });
          }
        });
      }, 10);
    },
  },
};
</script>

<style>
#odiv {
  width: 100vh;
  height: 100vh;
  background: #eee;
}
</style>