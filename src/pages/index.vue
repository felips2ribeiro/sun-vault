<template>
  <v-container fluid>
    <!-- Conteúdo principal -->
    <v-row >
      <!-- Primeira fileira de indicadores -->

      <v-col cols="12" md="">
        <v-skeleton-loader v-if="temperature === null" class="bg-transparent" type="card" />
        <v-card v-else class="mx-auto bg-transparent" flat height="100%">
          <v-card-item title="Brasil"></v-card-item>
          <v-card-text>
            <v-row align="center" no-gutters>
              <v-col class="text-h2 font-weight-medium text-grey-darken-2">{{ temperature }}°C </v-col>
              <v-col class="text-right">
              </v-col>
            </v-row>
          </v-card-text>

          <div class="d-flex py-3 justify-space-between">
            <v-list-item density="compact" prepend-icon="mdi-weather-windy">
              <v-list-item-subtitle>{{ windSpeed }} km/h</v-list-item-subtitle>
            </v-list-item>

            <v-list-item density="compact" prepend-icon="mdi-weather-pouring">
              <v-list-item-subtitle>12%</v-list-item-subtitle>
            </v-list-item>
          </div>
        </v-card>
      </v-col>

      <v-col cols="6" md="">
        <v-skeleton-loader v-if="temperature === null" class="bg-transparent" type="card" />

        <v-card v-else height="100%" flat class="d-flex flex-column align-center justify-center pa-4">
          <v-row align="center" justify="center" no-gutters>
            <v-col cols="auto" class="d-flex justify-center">
              <v-img src="@/assets/carbon.png" width="50px"></v-img>
            </v-col>
            <v-col cols="auto" class="d-flex justify-center">
              <p class="text-h6 ml-2 text-grey-darken-2 text-center">Carbono Evitado</p>
            </v-col>
          </v-row>

          <p class="text-h4 text-grey-darken-2 font-weight-bold text-center">15.3</p>
        </v-card>
      </v-col>

      <v-col cols="6" md="">
        <v-skeleton-loader v-if="temperature === null" class="bg-transparent" type="card" />

        <v-card v-else height="100%" flat class="d-flex flex-column align-center justify-center pa-4">
          <v-row align="center" justify="center" no-gutters>
            <v-col cols="auto" class="d-flex justify-center">
              <v-img src="@/assets/power.png" width="50px"></v-img>
            </v-col>
            <v-col cols="auto" class="d-flex justify-center">
              <p class="text-h6 ml-2 text-grey-darken-2 text-center">Potência Instantânea</p>
            </v-col>
          </v-row>

          <p class="text-h4 text-grey-darken-2 font-weight-bold text-center">3.2 kW</p>
        </v-card>
      </v-col>

      <v-col cols="6" md="">
        <v-skeleton-loader v-if="temperature === null" class="bg-transparent" type="card" />

        <v-card v-else height="100%" flat class="d-flex flex-column align-center justify-center pa-4">
          <v-row align="center" justify="center" no-gutters>
            <v-col cols="auto" class="d-flex justify-center">
              <v-img src="@/assets/production.png" width="50px"></v-img>
            </v-col>
            <v-col cols="auto" class="d-flex justify-center">
              <p class="text-h6 ml-2 text-grey-darken-2 text-center">Produção Total</p>
            </v-col>
          </v-row>

          <p class="text-h4 text-grey-darken-2 font-weight-bold text-center">245 kW</p>
        </v-card>
      </v-col>


      <v-col cols="6" md="">
        <v-skeleton-loader v-if="temperature === null" class="bg-transparent" type="card" />
        
        <v-card v-else height="100%" flat class="d-flex flex-column align-center justify-center pa-4">
          <v-row align="center" justify="center" no-gutters>
            <v-col cols="auto" class="d-flex justify-center">
              <v-img src="@/assets/refrigerator.png" width="50px"></v-img>
            </v-col>
            <v-col cols="auto" class="d-flex justify-center">
              <p class="text-h6 ml-2 text-grey-darken-2 text-center">Energia suficiente para:</p>
            </v-col>
          </v-row>

          <p class="text-h4 text-grey-darken-2 font-weight-bold text-center">120 Geladeiras</p>
        </v-card>
      </v-col>

    </v-row>

    <!-- Gráficos -->
    <v-row>
      <v-col cols="12" md="7">
        <v-card border="xl" class="pa-6" height="400px">
          <BarChart style="height: 100%;" />
        </v-card>
      </v-col>

      <v-col cols="12" md="5">
        <v-card border="xl" class="pa-6 text-center" height="400px">
          <p class="mb-2">Rendimento Mensal</p>
          <PieChart class="mx-auto" />
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import BarChart from '@/components/BarChart.vue';
import PieChart from '@/components/PieChart.vue';

// Variáveis para armazenar os dados climáticos
const temperature = ref(null);
const windSpeed = ref(null);
const humidity = ref(null);
const loading = ref(true);  // Controle de carregamento

// Função para buscar os dados da API
const fetchWeatherData = async () => {
  try {
    const response = await axios.get('https://api.open-meteo.com/v1/forecast', {
      params: {
        latitude: -23.5505,  // Latitude de São Paulo, Brasil
        longitude: -46.6333, // Longitude de São Paulo, Brasil
        current_weather: true,
        temperature_unit: 'celsius',
      }
    });

    const weatherData = response.data.current_weather;
    temperature.value = weatherData.temperature;
    windSpeed.value = weatherData.windspeed;
    humidity.value = weatherData.relative_humidity;

    loading.value = false;  // Dados carregados, parar carregamento
  } catch (error) {
    console.error('Erro ao buscar dados climáticos:', error);
    loading.value = false;  // Parar carregamento mesmo em erro
  }
};

// Chama a função ao montar o componente
onMounted(() => {
  fetchWeatherData();
});
</script>

<style scoped>
.v-card {
  border-radius: 8px;
}

.v-icon {
  color: #FF9800;
}

.text-center {
  text-align: center;
}
</style>
