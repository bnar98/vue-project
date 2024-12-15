<template>
  <div
    style="
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      position: relative;
    "
  >
    <canvas ref="chartCanvas"></canvas>
    <div
      class="circle-indicator"
      :style="`background-color: ${route.query.color ?? '#B2D2FE'};
          -webkit-box-shadow: 0px 0px 23px 2px ${route.query.color ?? '#B2D2FE'};
          -moz-box-shadow: 0px 0px 23px 2px ${route.query.color ?? '#B2D2FE'};
          box-shadow: 0px 0px 23px 2px ${route.query.color ?? '#B2D2FE'};`"
    >
      <div class="inner-content">
        <div>DAY</div>
        <div class="day-number">24</div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { Chart, DoughnutController, ArcElement, Tooltip, Legend, CategoryScale } from 'chart.js'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()
Chart.register(DoughnutController, ArcElement, Tooltip, Legend, CategoryScale)

const chartCanvas = ref<HTMLCanvasElement | null>(null)
let donutChart: any = null
// Define the shadow plugin
const shadowPlugin = {
  id: 'shadowPlugin',
  beforeDatasetsDraw(chart) {
    const { ctx } = chart

    chart.data.datasets.forEach((dataset, datasetIndex) => {
      const meta = chart.getDatasetMeta(datasetIndex)

      meta.data.forEach((arc, index) => {
        const model = arc.getProps(
          ['x', 'y', 'startAngle', 'endAngle', 'innerRadius', 'outerRadius'],
          true
        )

        ctx.save()

        // Set the shadow properties
        ctx.shadowColor = dataset.hoverBackgroundColor[index]
        ctx.shadowBlur = 20
        ctx.shadowOffsetX = 0
        ctx.shadowOffsetY = 0
        const shadowOffset = 1
        const shadowOuterRadius = model.outerRadius + shadowOffset
        ctx.beginPath()
        ctx.arc(model.x, model.y, shadowOuterRadius, model.startAngle, model.endAngle)
        // ctx.arc(model.x, model.y, model.outerRadius, model.startAngle, model.endAngle);
        ctx.arc(model.x, model.y, model.innerRadius, model.endAngle, model.startAngle, true)
        ctx.closePath()

        ctx.fillStyle = '#ffffff'
        ctx.fill()

        ctx.restore()
      })
    })
  }
}

// Register the plugin
Chart.register(shadowPlugin)

const data = {
  labels: [],
  datasets: route.query.chartData
    ? JSON.parse(route.query.chartData as string)
    : [
        {
          label: 'Colors',
          data: [route.query.period ?? 12, 20, 8],
          backgroundColor: ['#FF638450', '#36A2EB50', '#FFCE5650'],
          hoverBackgroundColor: ['#FF6384', '#36A2EB', '#FFCE56'],
          borderColor: ['#FF638450', '#36A2EB50', '#FFCE5650']
        }
      ]
}
const options = {
  responsive: true,
  maintainAspectRatio: false,
  cutout: '99.5%',
  layout: {
    padding: {
      top: 20,
      bottom: 20,
      left: 20,
      right: 20
    }
  }
}

//get today day
const today = new Date().getDate()
console.log(today)
//get number of days in this month
const daysInMonth = new Date(new Date().getFullYear(), new Date().getMonth() + 1, 0).getDate()
console.log(daysInMonth)

const centerTextPlugin = {
  id: 'centerText',
  afterDraw(chart) {
    const { ctx, chartArea } = chart
    const { width, height } = chartArea

    const centerX = (chart.chartArea.left + chart.chartArea.right) / 2
    const centerY = (chart.chartArea.top + chart.chartArea.bottom) / 2
    const radius = Math.min(width, height) / 2

    const progress = (today / daysInMonth) * 100
    const angle = (progress / 100) * 2 * Math.PI - Math.PI / 2
    const x = centerX + radius * Math.cos(angle)
    const y = centerY + radius * Math.sin(angle)

    ctx.save()
    ctx.beginPath()
    const circleRadius = 15 // Increase the size of the indicator by increasing this value
    ctx.arc(x, y, circleRadius, 0, 2 * Math.PI)
    ctx.fillStyle = 'white'
    ctx.lineWidth = 5 // Set the border width (2px)
    ctx.strokeStyle = '#76bff1' // Set the border color (red)
    ctx.stroke()
    ctx.fill()
    ctx.restore()
  }
}

// Register the center text plugin
// Chart.register(centerTextPlugin)
const createChart = () => {
  if (chartCanvas.value) {
    donutChart = new Chart(chartCanvas.value, {
      type: 'doughnut',
      data,
      options,
      plugins: [shadowPlugin]
    })
  }
}

onMounted(() => {
  createChart()
})

onBeforeUnmount(() => {
  if (donutChart) {
    donutChart.destroy()
  }
})
</script>

<style scoped>
canvas {
  width: 100%;
  height: 400px;
}

.circle-indicator {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height: 200px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-weight: bold;
}

.inner-content {
  text-align: center;
}

.day-number {
  font-size: 2em;
}
</style>
