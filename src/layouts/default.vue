<template>
  <v-app>
      <Sidebar />
      <v-main style="background-color: #edf2f8;">
        <Header style="background-color: #edf2f8;"/> 
        <router-view />
      </v-main>
    </v-app>
</template>

<script setup>
import Sidebar from '@/components/Sidebar.vue'
import Header from '@/components/Header.vue';
</script>

<style scoped>
/* Adicione estilos adicionais se necessário */
</style>
