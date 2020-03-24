<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <v-img
          :src="require('../assets/pokeball.jpg')"
          class="my-3"
          contain
          height="80"
        />
        <h1 class="display-2 font-weight-bold mb-3">
          POKEAPI CON VUE && VUETIFY
        </h1>        
      </v-col>
      <!-- Cards -->
      <v-col
        class="mb-5"
        cols="12"
      >
        <v-row justify="center">
        </v-row>
      </v-col>
<!-- buscador -->
      <v-col
        class="mb-5"
        cols="12"
      >
        
        <v-container fluid>
          <v-row>
            <v-col cols="12">
              <v-row 
                
                class="grey lighten-5"
                style="height: 300px;"
              >
                <v-card
                  v-for="(pokemon,index) in pokemons"
                  :key="'poke'+index"
                  class="mx-auto"
                  style="margin:15px"
                  max-width="250"
                  >
                  <v-img
                      style="background-color:#de8282"
                      class="align-end"
                      height="200px"
                      :src="images[index]"
                      >
                    <v-card-title style="color:yellow;">{{pokemon.name}}</v-card-title>
                  </v-img>
                    <v-card-subtitle class="pb-0">Número de pokedex:{{index+1}} </v-card-subtitle>
                    <v-card-actions>
                      <v-btn
                        color="orange"
                        text
                      >
                        Detalle del pokemon
                      </v-btn>
                    </v-card-actions>
                </v-card>                
              </v-row>
            </v-col>
          </v-row>      
        </v-container>
      </v-col>

    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
  export default {
    name: 'HelloWorld',
    data() {
      return { //https://assets.pokemon.com/assets/cms2/img/pokedex/detail/001.png
        apiURL:"https://pokeapi.co/api/v2/pokemon",
        imageURL:"https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
        pokemons:[],
        images:[],
        nextURL:"",
        npokedex:""
      }},
      methods: {
        async getPokemon(){
          let resultsArray = await axios.get(this.apiURL);
          let next = resultsArray.data.next;
          this.nextURL=next;
          this.pokemons=resultsArray.data.results;
          
          for( let Sindex = 0; Sindex <20;Sindex++){
            let Separado = resultsArray.data.results[Sindex].url.split(['/']);                 
            this.images.push(this.imageURL+Separado[6]+'.png');
          }
          for (let index = 0; index <(resultsArray.data.count)/20; index++) {
            if(this.nextURL!=null){
              
              let NextResults = await axios.get(this.nextURL);
              for(let i=0; i<NextResults.data.results.length;i++){
                this.pokemons.push(NextResults.data.results[i]);
              }
              this.nextURL=NextResults.data.next; 
            }
          }       
          for( let Sindex = 20; Sindex <(resultsArray.data.count);Sindex++){
            let Separado = resultsArray.data.results[Sindex].url.split(['/']);                 
            this.images.push(this.imageURL+Separado[6]+'.png');
          }
        }
      },
      created(){
        this.getPokemon();
        
      }    
  }
</script>
<!-- https://assets.pokemon.com/assets/cms2/img/pokedex/detail/001.png -->