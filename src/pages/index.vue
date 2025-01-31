<template>
  <v-container>
    <v-row >

      <v-col cols="12" md="3">
        <v-skeleton-loader v-if="temperature === null" class="bg-transparent" type="card" />
        <v-card v-else class="mx-auto bg-transparent" flat height="100%">
          <v-card-item title="Brasil"></v-card-item>
          <v-card-text>
            <v-row align="center">
              <v-col class="text-h2 font-weight-medium text-grey-darken-2">{{ temperature }}°C </v-col>
              <v-col class="text-right"></v-col>
            </v-row>
          </v-card-text>

          <v-container>
            <v-list-item density="compact" prepend-icon="mdi-weather-windy">
              <v-list-item-subtitle>{{ windSpeed }} km/h</v-list-item-subtitle>
            </v-list-item>

            <v-list-item density="compact" prepend-icon="mdi-weather-pouring">
              <v-list-item-subtitle>12%</v-list-item-subtitle>
            </v-list-item>
          </v-container>
        </v-card>
      </v-col>

      <v-col cols="6" md="">
        <CardIndicator icon="mdi-molecule-co2" title="Carbono Evitado" value="15.3 tCO²" color="#fef1c5" iconColor="#c6ad79" :loading="loading"/>
      </v-col>

      <v-col cols="6" md="">
        <CardIndicator icon="mdi-solar-power" title="Potência Instantânea" value="3.2 kW" color="#fbeff2" iconColor="#ffbfa2" :loading="loading"/>
      </v-col>

      <v-col cols="6" md="">
        <CardIndicator icon="mdi-solar-power-variant-outline" title="Produção Total" value="245 kW" color="#e5fff6" iconColor="#a3eabd" :loading="loading"/>
      </v-col>

      <v-col cols="6" md="">
        <CardIndicator icon="mdi-fridge-outline" title="Energia suficiente para:" value="120 Geladeiras" color="#f8eaff" iconColor="#d99dc5" :loading="loading"/>
      </v-col>

    </v-row>

    <v-row>

      <v-col cols="12" md="7">
        <BarChart/>
      </v-col>

      <v-col cols="12" md="5">
        <PieChart/>
        <v-card

          class="mx-auto"
          color="surface-variant"
          max-width="340"
          subtitle="17 Placas"
          title="Felipe Casa"
        >
          <template v-slot:actions>
            <v-btn
              append-icon="mdi-chevron-right"
              color="yellow-lighten-2"
              text="Editar"
              variant="outlined"
              block
            ></v-btn>
          </template>
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
import CardIndicator from '@/components/CardIndicator.vue';

const temperature = ref(null);
const windSpeed = ref(null);
const humidity = ref(null);
const loading = ref(true);

const fetchWeatherData = async () => {
  try {
    const response = await axios.get('https://api.open-meteo.com/v1/forecast', {
      params: {
        latitude: -23.5505,
        longitude: -46.6333,
        current_weather: true,
        temperature_unit: 'celsius',
      }
    });

    const weatherData = response.data.current_weather;
    temperature.value = weatherData.temperature;
    windSpeed.value = weatherData.windspeed;
    humidity.value = weatherData.relative_humidity;

    loading.value = false;
  } catch (error) {
    console.error('Erro ao buscar dados climáticos:', error);
    loading.value = false;
  }
};

onMounted(() => {
  fetchWeatherData();
});
</script>

<style scoped>


</style>
