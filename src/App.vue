<template>
  <div id="app">
    <div class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">Rick&Morty</span>
          <span class="subtitle">personajes</span>
          
        </h1>
        <div class="field has-addons is-pulled-right">
          <div class="control">
            <input type="text" class="input is-rounded" v-model="search" v-on:keyup.enter="searchData">
          </div>
          <div class="control">
            <button v-on:click="searchData" class="button is-success is-rounded">buscar</button>
          </div>
        </div>

        
      </div>
    </div>
<!-- ##########################################################3-->

    <div class="container">
      <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered">
        <character @showModal="showModal" v-for="character of characters" v-bind:key="character.id" v-bind:character="character"/>
      </div>


      <nav class="pagination" role="pagination" aria-label="pagination">
        <a class="pagination-previous" v-on:click="changePage(page-1)">Anterior</a>
        <ul>
          <li class="pagination-link is-current">{{page}}</li>
        </ul>
        <a class="pagination-next" v-on:click="changePage(page+1)">Siguiente</a>

        
      </nav>
    </div>




    <div class="modal" :class="{'is-active': modal}" v-if="modal">
      <div class="modal-background" @click="modal = false;"></div>
      <div class="modal-card">
      <header class="modal-card-head">
        <div class="modal-card-title"><p>Acerca de {{fastCharacter.name}}</p></div>
      </header>

      <div class="modal-card-body">
      <p>Genero: {{fastCharacter.gender}}</p>
      <p>Estado: {{fastCharacter.status}}</p>
      <p>Raza: {{fastCharacter.species}}</p>
      <p>Origen: {{fastCharacter.origin.name}}</p>
      </div>
      
      <footer class="modal-card-food">
        <button class="button" @click="modal = false;">Cerrar</button>
      </footer>
      </div>
    </div>
    
    
  
  </div>
</template>

<script>

import axios from 'axios';
import Character from './components/Character';

export default {
  name: "App",
  components:{
    Character //character
  },
  data: function(){
    return{
      characters:[],
      page:1,
      pages:1,
      search:"",
      modal: false,
      fastCharacter:{}
    }
  },

created (){
  this.fetch();
}
  ,
  methods:{
    async fetch(){   //ir a buscar

     const params={
       page:this.page,
       name:this.search
     };
      let result = axios.get("https://rickandmortyapi.com/api/character/",{params}) // let variable local 
      .then((res)=> {   //res respuesta y resive una funcion 
      this.characters = res.data.results;
      
      this.pages = res.data.info.pages;
     console.log(res.data); //paquete de datos en la libreria de axios  
      })

      .catch(err =>  { //cachar el error
        console.log(err); //imprimir el error
      })

    },

    changePage(page){
      this.page = (page <=0 || page >this.pages) ? this.page: page;
      this.fetch();

    },
    searchData(){
      this.page =1;
      this.fetch();
    },
    showModal(id){
this.fetchOne(id);
    },
    async fetchOne(id){
      let result = await axios.get(`https://rickandmortyapi.com/api/character/${id}/`)
      this.fastCharacter = result.data;
      this.modal= true;
      console.log(this.fastCharacter,"Personaje");
    }
  }
};
</script>

