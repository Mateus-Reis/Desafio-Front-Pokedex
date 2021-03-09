<template>
  <div id="pokemon">

    <div class="card" @click="openModal()">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title is-4">{{ num }} - {{ name }}</p>
          </div>
        </div>

        <div class="content">
            <button class="button is-fullwidth" @click="mudarSprite">Abrir Pokemon</button>
        </div>
      </div>
    </div>

    <div class="modal" :class="{'is-active': modalActived}">
       <div class="modal-background"></div>
       <div class="modal-content">
         
           <div class="card">
      <div class="card-image">
        <figure>
          <img :src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="subtitle is-5">{{ this.pokemon.type }}</p>
            <p class="subtitle is-5">{{ this.pokemon.ability }}</p>
            <p class="subtitle is-5">{{ this.pokemon.height }}</p>
            <p class="subtitle is-5">{{ this.pokemon.weight }}</p>


          </div>
        </div>

        <div class="content">
            <button class="button is-fullwidth" @click="mudarSprite">Girar Pokemon :)</button>
        </div>
      </div>
    </div>
         
       </div>
       <button class="modal-close" @click="closeModal()"></button> 
     </div>
   </div>
</template>

<script>
import axios from "axios";

export default {

watch: {
    textoBuscado: function ()  {
      console.log(this.pokemon);
      this.chamarPokemon();
    },
  },

  created: function () {
    this.chamarPokemon();
  },
  data() {
    return {
        modalActived: false,
        isFront: true,
        currentImg: '',
      pokemon: {
        type: "",
        front: "",
        back: "",
        ability: "",
        height: "",
        weight: "",
      },
    };
  },
  props: {
    textoBuscado: String,
    num: Number,
    name: String,
    url: String
  },

  methods: {

      openModal () {
          this.modalActived = true;
      },

      closeModal () {
        this.modalActived = false;
      },

      chamarPokemon () {
        axios.get(this.url).then((res) => {
        this.pokemon.type = "Tipo: " + res.data.types[0].type.name;
        this.pokemon.ability = "Habilidade: " + res.data.abilities[0].ability.name;
        this.pokemon.height = "Altura: " + res.data.height + " M";
        this.pokemon.weight = "Peso: " + res.data.weight + " Kg";
        this.pokemon.front = res.data.sprites.front_default;
        this.pokemon.back = res.data.sprites.back_default;
        this.currentImg = this.pokemon.front;
      
    });
    console.log(this.url);
      },
      mudarSprite () {
          if(this.isFront){
              this.isFront = false;
              this.currentImg = this.pokemon.back;
          }else {
              this.isFront = true;
              this.currentImg = this.pokemon.front;
          }
      }
  }
};

</script>

<style>
#pokemon {
    margin-top: 2%;
}
</style>
