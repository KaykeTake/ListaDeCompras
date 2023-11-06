<template>
  <div id="app">
    <ListaCompras :searchbtt="searchbtt" :pesquisa="pesquisa" :pesquisatag="pesquisatag" @recebendo="apagando"
    @buttonoff="buttonoff" @apagandotag="apagandotag"/>
    <PesquisandoLista ref="pesquisando" @recebendopesquisa="pesquisarlista" @recivebutton="searchbutton" />
    
  </div>
</template>

<script>

import ListaCompras from './components/ListaCompras.vue';
import PesquisandoLista from './components/Pesquisando.vue';

export default {
  name: 'App',
  components: {
    ListaCompras,
    PesquisandoLista
  },
  data() {
    return {
      pesquisatag: [],
      pesquisa: '',
      searchbtt: false,
    }
  },
  methods: {
    apagando() {
      this.$refs.pesquisando.limpando()
    },
    buttonoff() {
      this.searchbtt = false
    },
    pesquisarlista(data) {
      if (typeof data === 'string') {
        this.pesquisa = data
      }
      else if (Array.isArray(data)) {
        this.pesquisatag = data
      }
    },
    searchbutton() {
      this.searchbtt = true
    },
    apagandotag(){
      this.$refs.pesquisando.limpandotag()
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  justify-content: center;
  gap: 30px;
}
</style>
