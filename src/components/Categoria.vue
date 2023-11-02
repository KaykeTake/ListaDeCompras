<template>
    <div>
        <!-- o v-if esta verificando se o a variavel botao que vem do ListaCompras recebeu a variavel True -->
        <div class="pacote" v-if="botaoaba === true">
            <!-- uma leitura do arrai categorias feita pelo v-for para exibir na tela  -->
            <div class="box" v-for="item in categorias" :key="item.id">
                <div class="checkbox">
                    <input type="checkbox" :value='item' v-model="stags">
                </div>
                <label>{{ item }}</label>
            </div>
        </div>
    </div>
</template>
<script>

export default {
    name: "CategoriaComponent",
    components: {
    },
    props:{
        // armazena true ou false enviado por uma função no componente listacompras
        botaoaba: Boolean,
        clicks: Boolean
    },
    watch: {
        clicks(){
            if(this.clicks === true){
                this.$emit('receber', this.stags)
                this.stags = []
            }
        },
        stags(){
            this.$emit('receber', this.stags)
        }
    },
    data() {
        return {
            categorias: ["frutas", "legumes", "cereais", "laticinios", "bebidas"],
            stags: []
        };
    },
    methods: {
        limpando(){
            this.stags = []
        }
    },
}
</script>
<style scoped>
.pacote{
    display: flex;
    justify-content: flex-start;
    width: min-content;
    flex-wrap: wrap;
}
.box{
    display: flex;
    margin-right: 5px;
}
.checkbox{
    transform:translate(-50%, -50%) ;
}
input[type='checkbox']{
    position: relative;
    width: 40px;
    height: 18px;
    outline: none;
    -webkit-appearance: none;
    transition: .5s;
    background: #c6c6c6;
    border-radius: 20px;

}
input:checked[type='checkbox']{
    transition: .5s;
    background: #4228a3;
}
input[type='checkbox']::before{
    content: '';
    width: 18px;
    height: 18px;
    position: absolute;
    top: 0;
    left: 0;
    background: white;
    border-radius: 50%;
    box-shadow: 0 2px 5px rgb(157, 152, 152);
    transform:scale(1.2);
    transition: .5s;
    cursor: pointer;
}
input:checked[type='checkbox']::before{
    left: 20px;
}
</style>
