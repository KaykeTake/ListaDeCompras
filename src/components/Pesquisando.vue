<template>
    <div class="container">
        <CategoriaComponent ref="categoria" :botaoaba="btag" @receber="tag"/>
        <BotaoComponent  name="Tags" @receber="botaotag"/>
        <BotaoComponent name="Search" @receber="search"/>
        <BarraPesquisa ref="pesquisa" @recebendopesquisa="pesquisa"/>
    </div>
</template>
<script>
import BarraPesquisa from './BarraPesquisa.vue';
import BotaoComponent from './BotaoComponent.vue';
import CategoriaComponent from './Categoria.vue';

export default {
    name: "PesquisandoLista",
    components: { BotaoComponent, BarraPesquisa, CategoriaComponent },
    data(){
        return{
            btag: false,
            tags: []
        }
    },
    methods:{
        pesquisa(data){
            this.$emit('recebendopesquisa', data)
        },
        tag(data){
            this.tags = data
            this.$emit('recebendopesquisa', data)
        },
        search(){
            this.$emit('recivebutton')
        },
        limpando(){
            this.$refs.pesquisa.limpar()
        },
        limpandotag(){
            this.$refs.categoria.limpando()
        },
        botaotag(){
            this.btag = !this.btag
        },
    }
}
</script>
<style scoped></style>