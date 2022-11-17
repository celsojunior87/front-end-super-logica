<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      persistent
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Adicionar Usuário
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="text-h5">Adicionar Usuário</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                    v-model="name"
                    :counter="30"
                    :rules="nameRules"
                    label="Nome*"
                    required
                ></v-text-field>
              </v-col>
               <v-col cols="12">
                <v-text-field
                v-model="login"
                    :counter="10"
                    :rules="loginRules"
                    label="Login*"
                    required
                ></v-text-field>
              </v-col>
       
              <v-col cols="12">
                <v-text-field
                 v-model="email"
                 :rules="emailRules"
                 label="E-mail*"
                 required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                 v-model="zipcode"
                 :rules="zipCodeRules"
                 label="Cep*"
                 type="number"
                 required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                 v-model="password"
                 :rules="passwordRules"
                 label="Password*"
                 required
                 type="password"
                 error-count="5"
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="6"
              > 
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
            v-on:click="saveUsers()"
            :disabled="isDisabled"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
import axios from 'axios';

  export default {
    name: 'DialogUser',
      data: () => ({
      valid:false,  
      snackbar:{
         show:false,
         message:'',
         color:''   
      },
      messa:[],  
      dialog: false,
        message: '',
        timeout: 3000,

      name: '',
      nameRules: [
        v => !!v || 'Nome é Obrigatorio',
      ],
      login:'',
          loginRules: [
        v => !!v || 'Nome do Login é Obrigatório',
      ],
      email:'',
       emailRules: [
        v => !!v || 'E-mail é Obrigatorio',
        v => /.+@.+\..+/.test(v) || 'E-mail Não é Valido!!',
      ],
      zipcode:'',
        zipCodeRules: [
        v => !!v || 'O cep é Obrigatório',
      ],
      password:'',
      passwordRules:[
        v => !!v || 'O Password é Obrigatório',
        v => (v && /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/.test(v)) || 'Mínimo 8 caracteres uma letra é um numero',

      ],
    }),
    methods:{
        async saveUsers(){
           const form = {
               name: this.name,
               name_login:this.login,
               email:this.email,
               zipcode :this.zipcode,
               password:this.password
           }
           const response = axios
           .post('http://localhost:8000/api/store',form)
           .then((response) => {
               if(response.data.error == false){
                  this.$toast(response.data.message, {
                  timeout: 4000
                });
               }              
          })
        .catch((error) => { 
            this.erros = error.response.data.errors;
            Object.keys( this.erros).forEach(key => {
             this.$toast.error(this.erros[key], {
                  timeout: 4000
                }); 
            });               
         }); 
        }
    },
    computed:{
       isDisabled(){
       return !(this.name && this.passwordRules && this.login && this.zipcode && this.password && this.email)
      }
    },
  }
</script>