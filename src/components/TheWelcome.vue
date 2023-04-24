<script setup>
  import { onMounted, ref } from 'vue'
  import axios from 'axios'

  let pays = ref([]);
  let searchValue = ref('')
 
  onMounted(() => { 
    initialize();
  })

  function initialize() {
    axios
  .get('https://restcountries.com/v3.1/all')
  .then(
    (reponse) => {
      pays.value = reponse.data
      console.log(pays.value)
    }
  )
  .catch(erreur => console.log(erreur));
  }

  function search(){
    if(searchValue.value.trim().length > 0) {
      axios
      .get('https://restcountries.com/v3.1/name/' + searchValue.value)
      .then(
        (reponse) => {
          pays.value = reponse.data
          console.log(pays.value)
        }
        )
    }
  }

</script>

<template>
  <div class="container">
    <div class="row">
      <v-icon icon="fa:fas fa-search" color="primary"></v-icon>
      <v-text-field label="Search Countries..." v-model="searchValue" @keyup.enter="search"></v-text-field>
      <button @click="initialize" v-show="searchValue.trim().length > 0" id="but">Réinitialiser</button>
    </div>
    <v-row no-gutters>
      <v-col
      v-for="( pay, index) in pays"
      :key="index"
      cols="12"
      xs="12"
      sm="6"
      md="4"
      class="pa-1">
        <v-card  max-width="344" class="mx-auto pa-2 ma-1">
          <v-img 
          v-bind:src=" pay.flags.png "
          height="200px"
          cover>
                    
          </v-img>
          
          <v-card-title>
            {{ pay.name.common }}
          </v-card-title>
          
          <v-card-subtitle>
            Continent: {{ pay.continents }} {{ pay.timezones }}
          </v-card-subtitle> 
        </v-card>            
      </v-col>
    </v-row>
  </div>
</template>

<style scoped>
.container{
  width: 200%;
  margin: 0 auto;
}
#but{
  color: #cdbc7a;
    margin: 10px;
    padding: 13px;
    border-radius: 10px;
    border: 4px double #ccc;
}
</style>