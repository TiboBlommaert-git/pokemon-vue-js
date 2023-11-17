<template>
  <div class="c-pokemon">
    <img
      :src="getPokemonImageUrl()"
      :alt="`${PokemonData.name}`"
      class="c-pokemon__img"
    />
    <div class="c-pokemon__info">
      <header class="c-pokemon__header">
        <h2 class="c-pokemon__name">{{ PokemonData.name }}</h2>
        <label class="c-pokemon-spin" :for="PokemonData.name">
            <input class="c-pokemon-spin__input sr-only" type="checkbox" name="" :id="PokemonData.name" v-model="showBack">
            <RotateCw v-if="showBack" class="c-pokemon-spin__icon"/>
            <RotateCcw v-else class="c-pokemon-spin__icon"/>
        </label>
        <span class="sr-only">
          toggle pokemon from back to front and vice versa.
        </span>
      </header>
      <p class="c-pokemon__link"> more info <ChevronRight/></p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { RotateCcw, RotateCw, ChevronRight } from "lucide-vue-next";
const props = defineProps({
  PokemonData: {
    type: Object, //{}
    required: true,
  },
});

const showBack = ref(false)


const getPokemonId = function () {
  const PartsOfUrl = props.PokemonData.url.split("/");
  console.log(PartsOfUrl);
  const pokemonId = PartsOfUrl[PartsOfUrl.length - 2];
  console.log(pokemonId);
  return pokemonId;
};
getPokemonId();

const getPokemonImageUrl = function () {
  
  const baseUrl = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/`;
  if(showBack.value){
    return `${baseUrl}back/${getPokemonId()}.gif`
  }else{
    return `${baseUrl}${getPokemonId()}.gif`
  }
};
</script>
<style lang="scss">
.c-pokemon {
  width: 100%;
  @media (min-width: 480px) {
    width: calc((100% - 1*2rem)/2);
  }
  @media (min-width: 768px) {
    width: calc((100% - 3 * 2rem )/4);
  }
  &__info{
    display: flex;
    flex-direction: column;
    background-color: #444;
    color: #fff;
    padding: .5rem 1rem;
    border-radius: 1rem 2rem 1rem 2rem;

  }
  &__header{
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  &__name{
    font-size: 1.5rem;
    font-weight: 600;
    margin: 0;
    &:first-letter{
      text-transform: uppercase;
    }
  }
  &-spin{
    display: flex;
    flex-shrink: 0;
    align-items: center;
    justify-content: center;
    background-color: #111;
    border-radius: 50%;
    height: 3rem;
    width: 3rem ;
    cursor: pointer;
    margin-left: 1rem;
    transition: background 0.2s ease-in-out;
    color: white;
    &:hover{
      background-color: #808080;
    }
    &:focus-within{
      outline: 3px solid lightcyan;
    }
    &__input{
      position: absolute;
      widows: 1px;
      height: 1px;
      padding: 0;
      margin: -1px;
      overflow: hidden;
      clip: rect(0,0,0,0);
      white-space: nowrap;
      border-width: 0;
    }
  }
  &__img{
    height: 7rem;
    width: auto;
    display: block;
    margin-bottom: -0.5rem;
  }
  &__link{
    padding: 1rem 0;
    font-size: .75rem;
    margin: 0 0 0 auto;
    text-decoration: underline;
  }
}

.sr-only{
      position: absolute;
      widows: 1px;
      height: 1px;
      padding: 0;
      margin: -1px;
      overflow: hidden;
      clip: rect(0,0,0,0);
      white-space: nowrap;
      border-width: 0;
}
</style>
