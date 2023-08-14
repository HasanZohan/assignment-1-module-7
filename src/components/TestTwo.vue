<script setup>
  import { onMounted, onBeforeUnmount, ref } from 'vue';
  import Chart from 'chart.js/auto';
  
  let chartInstance = null; // Store the Chart.js instance
  const chartData = ref({
    labels: ['Red', 'Blue', 'Yellow'],
    data: [300, 50, 100],
    backgroundColor: ['rgb(255, 99, 132)', 'rgb(54, 162, 235)', 'rgb(255, 205, 86)'],
  });
  const newType = ref('');
  const newValue = ref('');
  
  onMounted(() => {
    renderChart();
  });
  
  onBeforeUnmount(() => {
    if (chartInstance) {
      chartInstance.destroy();
    }
  });
  
  function renderChart() {
    const ctx = document.getElementById('chart');
    if (chartInstance) {
      chartInstance.destroy();
    }
    chartInstance = new Chart(ctx, {
      type: 'pie',
      data: {
        labels: chartData.value.labels,
        datasets: [{
          data: chartData.value.data,
          backgroundColor: chartData.value.backgroundColor,
        }],
      },
    });
  }
  
  function addData() {
    if (newType.value && newValue.value) {
      chartData.value.labels = [...chartData.value.labels, newType.value];
      chartData.value.data = [...chartData.value.data, newValue.value];
      chartData.value.backgroundColor = [...chartData.value.backgroundColor, getRandomColor()];
  
      renderChart();
  
      newType.value = '';
      newValue.value = '';
    }
  }
  
  function getRandomColor() {
    return `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)})`;
  }
</script>

<template>
    <div class="container">
      <div class="row d-flex justify-content-center">
        <div class="col-md-6">
          <canvas id="chart" width="400" height="400"></canvas>
        </div>
      </div>
      <div class="row d-flex justify-content-center">
        <div class="col-md-6">
          <div class="input-container">
            <label for="newType" class="input-label">Type:</label>
            <input v-model="newType" id="newType" class="input-field" placeholder="Enter type">
            <label for="newValue" class="input-label">Value:</label>
            <input v-model.number="newValue" id="newValue" class="input-field" placeholder="Enter value">
            <button @click="addData" class="add-button">Add Data</button>
          </div>
        </div>
      </div>
    </div>
</template>

<style>
.container {
  margin-top: 50px;
}

.input-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-top: 20px;
}

.input-label {
  margin-bottom: 5px;
}

.input-field {
  margin: 5px 0;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
}

.add-button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 4px;
  cursor: pointer;
  width: 100%;
  transition: background-color 0.3s;
}

.add-button:hover {
  background-color: #0056b3;
}
</style>