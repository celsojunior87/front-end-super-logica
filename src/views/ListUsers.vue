<template>
  <v-card>
    <v-card-title>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Pesquisar"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="users"
      :search="search"
      no-results-text="Nenhum registro encontrado"
	   :footer-props="{
        'items-per-page-options': [10, 20, 30, 40, 50]
     }"
    :items-per-page="10"
    >
    </v-data-table>
        <dialog-user />
  </v-card>
</template>
<script>

import DialogUser from '../components/DialogUser';
import axios from 'axios';

  export default {
    components: {
      DialogUser,
    },
    data () {
      return {
          users:[],
        search: '',
        headers: [
          { text: 'Nome', value: 'name', align: 'center' },
          { text: 'Login', value: 'name_login', align: 'center' },
          { text: 'E-mail', value: 'email', align: 'center' },
          { text: 'Zipcode', value: 'zipcode', align: 'center'},
    
        ],
      }      
    },
    mounted(){
        this.getUsers();
 },
 methods:{
    async getUsers() {
        try {
         const response = await axios.get('http://localhost:8000/api/list');
          this.users = response.data.results;
        }catch (error) {
        console.error(error);
    }
     }
 }
  }
</script>