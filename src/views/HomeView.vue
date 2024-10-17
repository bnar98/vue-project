<template>
  <canvas ref="chartCanvas">
  </canvas>
  <div style="display: flex;justify-content: center;">

    <div style="margin:82px;width:260px; height:260px;
       background-color: #f2f2f2;position: absolute;
       z-index: -1000;top: 0;
       border-radius: 50%;
       ">

    </div>
  </div>

</template>

<script lang="ts" setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { Chart, DoughnutController, ArcElement, Tooltip, Legend, CategoryScale } from "chart.js";

Chart.register(DoughnutController, ArcElement, Tooltip, Legend, CategoryScale);

const chartCanvas = ref<HTMLCanvasElement | null>(null);
let donutChart: Chart | null = null;
// Define the shadow plugin
const shadowPlugin = {
  id: 'shadowPlugin',
  beforeDatasetsDraw(chart) {
    const { ctx } = chart;

    chart.data.datasets.forEach((dataset, datasetIndex) => {
      const meta = chart.getDatasetMeta(datasetIndex);

      meta.data.forEach((arc, index) => {
        const model = arc.getProps(
          ['x', 'y', 'startAngle', 'endAngle', 'innerRadius', 'outerRadius'],
          true
        );

        ctx.save();

        // Set the shadow properties
        ctx.shadowColor = dataset.hoverBackgroundColor[index];
        ctx.shadowBlur = 20;
        ctx.shadowOffsetX = 0;
        ctx.shadowOffsetY = 0;
        const shadowOffset = 1;
        const shadowOuterRadius = model.outerRadius + shadowOffset;
        ctx.beginPath();
        ctx.arc(model.x, model.y, shadowOuterRadius, model.startAngle, model.endAngle);
        // ctx.arc(model.x, model.y, model.outerRadius, model.startAngle, model.endAngle);
        ctx.arc(model.x, model.y, model.innerRadius, model.endAngle, model.startAngle, true);
        ctx.closePath();

        ctx.fillStyle = '#ffffff';
        ctx.fill();

        ctx.restore();
      });
    });
  },
};

// Register the plugin
Chart.register(shadowPlugin);

const data = {
  labels: [],
  datasets: [
    {
      label: "Colors",
      data: [12, 20, 8],
      backgroundColor: ["#FF638450", "#36A2EB50", "#FFCE5650"],
      hoverBackgroundColor: ["#FF6384", "#36A2EB", "#FFCE56"],
      borderColor: ["#FF638450", "#36A2EB50", "#FFCE5650"],
    },
  ],
};

const options = {
  responsive: true,
  maintainAspectRatio: false,
  cutout: '99.5%',
  layout: {
    padding: {
      top: 20,
      bottom: 20,
      left: 20,
      right: 20,
    },
  },

};
const centerTextPlugin = {
  id: "centerText",
  afterDraw(chart) {
    const { ctx, chartArea } = chart;
    const { width, height } = chartArea;

    const centerX = (chart.chartArea.left + chart.chartArea.right) / 2;
    const centerY = (chart.chartArea.top + chart.chartArea.bottom) / 2;
    const radius = Math.min(width, height) / 2;

    const progress = 0.75;
    const angle = progress * 2 * Math.PI - Math.PI / 2;
    const x = centerX + radius * Math.cos(angle);
    const y = centerY + radius * Math.sin(angle);

    ctx.save();
    ctx.beginPath();
    const circleRadius = 15; // Increase the size of the indicator by increasing this value
    ctx.arc(x, y, circleRadius, 0, 2 * Math.PI);
    ctx.fillStyle = "white";
    ctx.lineWidth = 5; // Set the border width (2px)
    ctx.strokeStyle = "#76bff1"; // Set the border color (red)
    ctx.stroke();
    ctx.fill();
    ctx.restore();
  },
};

// Register the center text plugin
Chart.register(centerTextPlugin);
const createChart = () => {
  if (chartCanvas.value) {
    donutChart = new Chart(chartCanvas.value, {
      type: "doughnut",
      data,
      options,
      plugins: [shadowPlugin, centerTextPlugin],

    });
  }
};

onMounted(() => {
  createChart();
});

onBeforeUnmount(() => {
  if (donutChart) {
    donutChart.destroy();
  }
});
</script>

<style scoped>
canvas {
  width: 100%;
  height: 400px;
}
</style>
