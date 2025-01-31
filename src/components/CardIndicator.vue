<template>
    <v-skeleton-loader v-if="loading" class="bg-transparent" type="card" />
    <v-card v-else rounded="xl" height="100%" flat class="d-flex flex-column align-center justify-center pa-4">
        <v-row align="center" justify="center" no-gutters>
            <v-col  cols="4" :style="{backgroundColor: color}" class="pa-4 rounded-xl" >
                <v-icon :color="iconColor" size="x-large" :icon="icon"></v-icon>
            </v-col>
            <v-col cols="8" class="d-flex justify-center">
                <p class="text-h6 ml-2 text-grey-darken-2 text-center">{{ title }}</p>
            </v-col>
        </v-row>

        <p class="text-h4 text-grey-darken-2 font-weight-bold text-center">{{ value }}</p>
    </v-card>
</template>
  
<script>
import { color } from 'chart.js/helpers';

export default {
    props: {
        icon: String,
        title: String,
        value: String,
        color: String,
        iconColor: String,
        loading: Boolean
    },
};
</script>
  