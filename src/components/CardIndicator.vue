<template>
    <v-card rounded="xl" height="100%" flat class="d-flex flex-column align-center justify-center pa-4">
        <v-row align="center" justify="center" no-gutters>
        <v-col cols="auto" class="d-flex justify-center">
            <v-img :src="getImageUrl(icon)" width="50px"></v-img>
        </v-col>
        <v-col cols="auto" class="d-flex justify-center">
            <p class="text-h6 ml-2 text-grey-darken-2 text-center">{{ title }}</p>
        </v-col>
        </v-row>

        <p class="text-h4 text-grey-darken-2 font-weight-bold text-center">{{ value }}</p>
    </v-card>
</template>
  
<script>
export default {
    props: {
        icon: String, // Nome do arquivo de ícone (ex: "carbon.png")
        title: String, // Título do card
        value: String, // Valor a ser exibido no card
    },
    methods: {
        getImageUrl(icon) {
            return new URL(`/src/assets/${icon}`, import.meta.url).href;
        }
    }
};
</script>
  