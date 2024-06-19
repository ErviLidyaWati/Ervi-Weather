<template>
    <div class="temperature-chart">
      <line-chart :chart-data="chartData" :options="chartOptions"></line-chart>
      <p class="chart-title">Grafik Suhu Harian (°C)</p>
    </div>
  </template>
  
  <script>
  import { Line } from 'vue-chartjs';
  
  export default {
    extends: Line,
    props: {
      chartData: {
        type: Object,
        required: true
      },
      chartOptions: {
        type: Object,
        default: () => ({
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero: false
              }
            }]
          }
        })
      }
    },
    mounted() {
      this.renderChart(this.chartData, this.chartOptions);
    }
  };
  </script>
  
  <style scoped>
  .temperature-chart {
    margin-top: 20px;
    text-align: center;
  }
  
  .chart-title {
    font-size: 1.2rem;
    font-weight: bold;
  }
  </style>
  