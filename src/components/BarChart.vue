<template>
    <div class="chart-container">
      <div>{{ mes }}Mes</div>
      <BarChart :data="chartData" :options="chartOptions"/>
    </div>
  </template>
  
  <script>
  import { defineComponent, ref, onMounted } from 'vue';
  import { Bar } from 'vue-chartjs';
  import axios from 'axios';
  import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

  ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);
  
  export default defineComponent({
    components: {
      BarChart: Bar
    },
    setup() {
      const chartData = ref({
        labels: [], // Labels dos dias
        datasets: [
          {
            
            label: 'Energia Gerada (kW)',
            backgroundColor: '#42A5F5',
            data: [] // Dados da energia gerada
          }
        ]
      });

      const chartOptions = ref({
        plugins: {
            legend: {
                display: false
            }
        },
        responsive: true,
        maintainAspectRatio: false,
        cutout: '50%' // Faz um buraco no meio para mostrar o valor
      });
      
    const fetchData = async () => {
    try {
        const response = await axios.get('http://localhost:5000/energia')
        const jsonData = response.data

        const labels = jsonData.map(item => item.date)
        const energyData = jsonData.map(item => item.energy)

        chartData.value = {
            labels: labels,
            datasets: [
            {
                label: 'Energia Gerada (kW)',
                backgroundColor: '#42A5F5',
                data: energyData
            }
            ]
        };


    } catch (error) {
        console.error('Erro ao consumir a API', error)
    }
    }

    onMounted(() => {
      fetchData();
    });
    return { chartData };
    }
  });
  </script>
  
  <style scoped>
  /* Estilos personalizados para o gráfico */
  </style>
  