<template>
  <v-app>    
    <v-container>      
      <v-row class="text-center">
        <v-col cols="6">
          <h2>You</h2>
        </v-col>
        <v-col cols="6">
          <h2>Monster</h2>
        </v-col>
      </v-row>    
      <v-row class="text-center">
        <v-col>{{ health }}</v-col>
        <v-col>{{ monsterHealth }}</v-col>
      </v-row>
      <v-row class="text-center">
        <v-col>
          <v-btn id="attack" >Attack</v-btn>
        </v-col>
        <v-col>
          <v-btn color="primary" id="specialAttack">Special Attack</v-btn>
        </v-col>
        <v-col>
          <v-btn color="success" id="heal">Heal</v-btn>
        </v-col>
        <v-col>
          <v-btn color="error" id="surrender">Surrender</v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-app>  
</template>

<script>
  export default {
    name: "App",  
    components: {
      
    },
    data: () => ({
      health: 100,
      monsterHealth: 100
    })
  };
</script>

<style>

</style>