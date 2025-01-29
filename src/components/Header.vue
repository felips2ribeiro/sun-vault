<!-- /components/Header.vue -->
<template>
    <v-app-bar app flat>
      <v-spacer></v-spacer>
  
      <!-- Foto de perfil -->
      <v-avatar size="40px">
        <v-img
            alt="Avatar"
            src="https://avatars.githubusercontent.com/u/139786186?s=400&u=9bedf714f7993112168439604ed35a2eba2c163e&v=4"
            ></v-img>
      </v-avatar>
  
      <!-- Botão de mudar de tema -->
      <v-btn icon @click="toggleTheme">
        <v-icon>mdi-theme-light-dark</v-icon>
      </v-btn>
  
      <!-- Botão de sair -->
      <v-btn icon @click="logout">
        <v-icon>mdi-logout</v-icon>
      </v-btn>
    </v-app-bar>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  
  const toggleTheme = () => {
    // Lógica para alternar entre os temas (ex: claro/escuro)
    document.body.classList.toggle('dark-theme')
  }
  
  const logout = () => {
    // Lógica para fazer logout
    console.log('Usuário saiu!')
  }
  </script>
  
  <style scoped>
  /* Aqui você pode adicionar estilos adicionais para o Header */
  </style>
  