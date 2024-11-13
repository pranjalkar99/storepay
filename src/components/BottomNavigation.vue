<template>
    <v-bottom-navigation color="white" grow>
      <v-btn href="/" icon>
        <v-icon>mdi-home</v-icon>
      </v-btn>
      <v-btn href="category" icon>
        <v-icon>mdi-grid</v-icon>
      </v-btn>
      <v-btn href="cart" icon>
        <v-icon>mdi-cart</v-icon>
      </v-btn>
      
      <v-btn href="profile" icon>
        <v-icon>mdi-account</v-icon>
      </v-btn>
    </v-bottom-navigation>
  </template>
  
  <script>
  export default {
    name: "BottomNavigation",
  };
  </script>
  