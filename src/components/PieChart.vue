<template>
    <v-container>
      <v-card rounded="xl" flat style="border: 8px solid white;"  max-width="900" max-height="479">
        <Doughnut :data="chartData" :options="chartOptions" />
        <div class="chart-value mt-4 text-h4">${{ totalValor }}</div>
      </v-card>

      
    </v-container>
  </template>
  
  <script>
  import { defineComponent, ref, onMounted } from 'vue';
  import { Doughnut } from 'vue-chartjs';
  import { Chart as ChartJS, Title, Tooltip, Legend, ArcElement, CategoryScale } from 'chart.js';
  import axios from 'axios';
  
  ChartJS.register(Title, Tooltip, Legend, ArcElement, CategoryScale);
  
  export default defineComponent({
    components: { Doughnut },
    setup() {
      const chartData = ref({
        labels: [],
        datasets: [
          {
            label: 'Valor Gerado (R$)',
            backgroundColor: ['#FFC107', '#4CAF50'], // Cores amarelo e verde
            data: [300,50,100]
          }
        ]
      });
  
      const totalValor = ref(0);
  
      const chartOptions = ref({
        plugins: {
            legend: {
                display: true,

            }
        },
        responsive: true,
        maintainAspectRatio: false,
        cutout: '50%' // Faz um buraco no meio para mostrar o valor
      });
  
      const fetchData = async () => {
        try {
          const response = await axios.get('http://localhost:5000/valor');
          const jsonData = response.data;
  
          const labels = jsonData.map(item => item.date);
          const valores = jsonData.map(item => item.value);
  
          totalValor.value = valores.reduce((acc, cur) => acc + cur, 0); // Soma total
  
          chartData.value = {
            labels: labels,
            datasets: [
              {
                label: 'Valor Gerado (R$)',
                backgroundColor: ['#FFC107', '#4CAF50'], // Amarelo e verde
                data: valores
              }
            ]
          };
        } catch (error) {
          console.error('Erro ao buscar dados da API', error);
        }
      };
  
      onMounted(() => {
        fetchData();
      });
  
      return { chartData, chartOptions, totalValor };
    }
  });
  </script>
  
  <style scoped>
  .chart-container {
    position: relative;
    width: 200px;
    height: 200px;
  }
  
  .chart-value {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 20px;
    font-weight: bold;
    color: #333;
  }
  </style>
  