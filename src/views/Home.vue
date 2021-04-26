<template>
<div id="pokedex">
    <div class="search">
      <div class="searchinput">
         <input v-model="search" placeholder="Pokemon Name" @keyup.enter="findByName">
      </div>
      <div class="searchbtn">
        <button @click="findByName">SEARCH</button>
      </div>
    </div>
    <img src="../assets/images/Pokédex_logo.png"  height= "150" width= "270" alt="" style="object-fit:contain">
    <div class="dex">     
        <div class="pokedeimage">
           <img src="../assets/images/pokedex.png" width= "700"  alt="">
        </div>     
        
         <div class="pokeimage">
           <img :src="poke_image_url" width= "120" alt="Pokeball">
           <h3 v-if="notFound">Pokemon Not Found!</h3>
         </div>

         <div class="pokenext">
           <button @click="nextPokemon"></button>
         </div>
          <div class="pokeprev">
           <button @click="previusPokemon"></button>
         </div>
         <div class="pokename">
           <h3>{{pokename}}</h3>
         </div> 
         <div class="poketype1">
           <h3>{{poketype1}}</h3>
         </div> 
         <div class="poketype2">
           <h3>{{poketype2}}</h3>
         </div> 
         <div class="pokestats">
          <span v-for="status in pokestats" v-bind:key="status.id">
            {{status.stat.name}} : {{status.base_stat}}
            <br />
          </span>
        </div>   
      
         
    </div>
  
</div>
  
</template>

<script>
// @ is an alias to /src
import axios from "axios"

export default {
  name: 'Home',
  data() {
    return{
      poke_image_url:require('@/assets/images/pokeball.png'),
      pokename:"NAME",
      poketype1:"TYPE1",
      poketype2:"TYPE2",
      pokeimage:"",
      pokestats:[{
        stat:{
          name:"STATUS NAME"
        },
        base_stat:"0"
      }],
      pokeId:"",
      search:"",
      notFound:false
    }
  },
  methods: {
    nextPokemon() {
      this.notFound=false
      this.find(this.pokeId + 1);
    },
    previusPokemon() {
      if(this.pokeId == 1){
        this.notFound=false
        return
      }
      this.find(this.pokeId - 1);
      this.notFound=false
    },

    findByName() {
      this.notFound=false
      this.find(this.search);
      
    },

    async find(param) {
      await axios
        .get("https://pokeapi.co/api/v2/pokemon/" + param)
        .then((resp) => {
          this.poke_image_url = "https://pokeres.bastionbot.org/images/pokemon/" + resp.data.id +".png";
          this.pokeId = resp.data.id;
          this.pokestats = resp.data.stats;
          this.pokename = resp.data.name;
          this.poketype1 = resp.data.types[0].type.name;
          if (resp.data.types[1]) {
            this.poketype2 = resp.data.types[1].type.name;
          } else {
            this.poketype2 = "";
          }
        })
        .catch((err) => {
          console.log(err);
          this.poke_image_url =require("@/assets/images/notFound.png");
          this.pokestats = "";
          this.pokename = "";
          this.poketype1 = "";
          this.poketype2 = "";
          this.notFound=true;
        });
    },
  },

 
}
</script>

<style >



#pokedex{
  display: flex;
  flex-direction: column;
  align-items: center;
  font-family: DMSans;
  justify-items: center;
}

.search{
  align-items: center;
  position: relative;
 justify-content: center;
 display: flex;

}
.searchbtn button{
 position: absolute;
 top: 140px;
 left:50px;
 height: 35px;
 border: none;
 width: 90px;
 font-family: DMSans;
 background:#d30a40;
  box-shadow: 4px 4px rgb(161, 4, 36),  3px 3px rgb(161, 4, 36), 2px 2px rgb(161, 4, 36), 1px 1px rgb(161, 4, 36);
}
.searchbtn button:hover{
  cursor: pointer;
}
.searchbtn button:active{
  box-shadow: 0 0 0; 
  transform: translate(4px, 4px);
}
.searchinput input{
  position: absolute;
  font-family: DMSans;
  top: 140px;
  height: 35px;
  width: 230px;
  right: 0;
  padding: 0 10px;
  margin-left:10px;
  border: none;
  border-radius: 2px;
  box-shadow: 1px 2px 2px rgba(0, 0, 0, 0.2);
}


.dex{
  position: relative;
  align-items: center;
  top:50px
}

.pokedeximage{
  position: absolute;
  top: 0;
  left:200px;
}
 
.pokeimage{
    position: absolute;
    top: 186px;
    left:130px;
}
.pokeimage h3{
 
  left: 0px;
  font-size: 15px;
  width: 150px;
}

.pokenext button{
  position: absolute;
  height: 20px;
  width: 29px;
  top:406px;
  left: 290px;
  opacity: 0;
  cursor: pointer;
}

.pokeprev button{
  position: absolute;
  height: 20px;
  width: 29px;
  top:406px;
  left: 245px;
  opacity: 0;
  cursor: pointer;
}
.pokename{
   position: absolute;
   top:409px;
  left: 107px;
  color: #084035;
  width: 105px;
  text-transform: uppercase;
  font-size: 13px;
  text-align: center;
  padding-top: 2%;
}

.poketype1 {
  position: absolute;
  top: 422px;
  width: 97px;
  color: #63c27f;
  left: 420px;
  text-transform: uppercase;
  font-size: 11px;
  text-align: center;
  padding-top: 7px;
  text-align: center;
}

.poketype2 {
  position: absolute;
  top: 422px;
  left: 536px;
  color: #63c27f;
  width: 97px;
  text-transform: uppercase;
  font-size: 11px;
  text-align: center;
  padding-top: 7px;
}

.pokestats {
  left: 423px;
  list-style-type: none;
  position: absolute;
  color: #63c27f;
  top: 202px;
  width: 224px;
  height: 60px;
  display: grid;
  grid-template-columns: 50% 50%;
  text-align: left;
  padding: 0px;
  font-size: 11px;
}


 
</style>
