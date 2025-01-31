<template>
    <v-footer
      class="text-center d-flex flex-column"
      style="background-color: transparent; max-height: 30px;"
    >
      <v-row justify="center">
        <v-col class="d-flex justify-center">
          <v-btn
            v-for="icon in icons"
            :key="icon.name"
            :icon="icon.icon"
            class="mx-4"
            variant="text"
            :href="icon.link"
            target="_blank"
          ></v-btn>
        </v-col>
      </v-row>
    </v-footer>
  </template>
  
  <script>
  export default {
    data: () => ({
      icons: [
        { icon: 'mdi-github', link: 'https://github.com/felips2ribeiro' },
        { icon: 'mdi-briefcase-account', link: 'https://felipe-portfolio-sigma.vercel.app/' },
        { icon: 'mdi-linkedin', link: 'https://br.linkedin.com/in/feliperibeirosoftware' },
        { icon: 'mdi-instagram', link: 'https://instagram.com/_ribeiroofelipe' },
      ],
    }),
  }
  </script>
  