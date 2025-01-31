<template>
  <v-container>
    <v-card color="#f1f4fe" rounded="xl" flat style="border: 8px solid white;"  max-width="900" max-height="479">
      <v-card-text>
        <v-select v-model="selectedMonth" :items="months" label="Selecione o mês" ></v-select>
        <v-sheet color="#f1f4fe">
          <v-sparkline :model-value="chartData" :gradient="['white', 'orange', 'red']" height="120" padding="24" line-width="2" smooth fill>
            <template v-slot:label="item">
              {{ item.value }} kWh
            </template>
          </v-sparkline>
        </v-sheet>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
import axios from "axios";


export default {
  data() {
    return {
      selectedMonth: "janeiro", // Default to "janeiro"
      months: [],
      energia: {}, // Stores the energy data for all months
    };
  },
  computed: {
    chartData() {
      const selectedMonthData = this.energia[this.selectedMonth] || [];

      const firstFourWeeksData = selectedMonthData.slice(0, 4);
      
      return firstFourWeeksData.map(item => item.energy);
    }
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        const response = await axios.get("http://localhost:5000/energia");
        
        // Assuming the structure of response data is the same as the one you provided
        // Store the data under 'energia' with months as keys
        
        this.energia = response.data;
        this.months = Object.keys(this.energia);
      } catch (error) {
        console.error("Erro ao buscar os dados de energia:", error);
      }
    }
  }
};
</script>
