<template>
    <div class="container">
        <!-- envia para o componente Categoria um boolean para execução da aparição e remosão da aba tags -->
        <BotaoComponent name="Tags" @receber="botaotag" />
        <!-- faz a comunicação com o botão Tags e lhe da um nome -->
        <BotaoComponent name="Add" @receber="botaoadd" />
        <CategoriaComponent :botaoaba="btag" :clicks="badd" @receber="addlisttag" />
        <BarraPesquisa :clicks="badd" @receber="addlistname" />
        <div class="box">
            <!-- v-for percorre o array list e exibir os valores de name e tag -->
            <div v-for="(item, index) in list" :key="index" :class="{ pesquisa: highlight(item) }">
                <a href="#" @click.prevent="excluir(index)">excluir</a> 
                <p> {{ item.name }}</p>
                <div v-for="(tag, index) in item.tag" :key="index"> <!-- exibi as tags de cada objeto -->
                    <p>{{ tag }}</p>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import BarraPesquisa from './BarraPesquisa.vue';
import BotaoComponent from './BotaoComponent.vue';
import CategoriaComponent from './Categoria.vue';

export default {
    name: "ListaCompras",
    components: {
        CategoriaComponent,
        BarraPesquisa,
        BotaoComponent
    },
    props: {
        pesquisa: String,
        searchbtt: Boolean,
        pesquisatag: Array
    },
    mounted(){
        this.carregamento()
    },
    watch: {
        pesquisa() {
            this.pesquisando = this.pesquisa
            if (this.pesquisando === '' && this.pesquisatag.length <= 0) {
                this.list = structuredClone(this.listbase)
            }
        },
        pesquisatag() {
            if (this.pesquisando === '' && this.pesquisatag.length <= 0) {
                this.list = structuredClone(this.listbase)
            }
        },
        searchbtt() {
            if (this.searchbtt === true) {
                this.searched()
                this.$emit('buttonoff',)
            }
        }
    },
    data() {
        return {
            listbase: [],
            list: [],
            listalterado: [],
            name: '',
            tag: [],
            btag: false,
            badd: false,
            count: 0,
            pesquisando: ''
        }
    },
    methods: {
        excluir(index){
            this.listbase.splice(index,1)
            localStorage.setItem('list', JSON.stringify(this.listbase))
            this.list = (structuredClone(this.listbase))
        },
        carregamento(){
            this.listbase = JSON.parse(localStorage.getItem('list')) || [];
            this.list = structuredClone(this.listbase)
        },
        highlight(item) {
            const tagMatch = item.tag.some(tag => {
                return this.pesquisatag.some(searchTag => tag.toLowerCase().includes(searchTag.toLowerCase()));
            });
            if (this.pesquisando === '') {
                return tagMatch;
            }
            const nameMatch = item.name.toLowerCase().includes(this.pesquisando.toLowerCase());
            return nameMatch || tagMatch;
        },
        searched() {
            this.list = this.listbase.filter(item => {
                // Verifica se pelo menos uma tag corresponde a pesquisatag
                const tagMatch = item.tag.some(tag => {
                    return this.pesquisatag.some(searchTag => tag.toLowerCase().includes(searchTag.toLowerCase()));
                });
                if (this.pesquisando === '') {
                    return tagMatch;
                }
                // Verifica se o nome corresponde à pesquisa
                const nameMatch = item.name.toLowerCase().includes(this.pesquisando.toLowerCase());
                // Retorna true se o nome corresponde ou pelo menos uma tag corresponde a pesquisatag
                return nameMatch || tagMatch;
            });
        },
        // adiciona um novo novo conjuto no arrray list, o .trim() bloquea a entrada de escrita em branco
        addlistname(data) {
            if (data.trim() === "") {
                this.badd = false;
                return;
            }
            this.listbase.unshift({
                name: data,
                tag: this.tag, // Adiciona a tag ao objeto na lista
            });
            localStorage.setItem('list', JSON.stringify(this.listbase))
            this.badd = false;
            this.name = ""; // Limpa o campo de entrada de nome
            this.tag = []; // Limpa o campo de entrada de tag
            this.list = structuredClone(this.listbase)
            this.$emit('apagandotag')
            this.pesquisando = ''
            this.$emit('recebendo')
        },
        addlisttag(data) {
            if (data.length === 0) {
                return;
            }
            this.tag = data; // Define a tag para ser adicionada ao próximo nome
        },
        // executa uma logica onde o primeiro click torna a varivel true e o segundo em false e envia para  o componente Categoria, fazendo a aba tags aparacer e sumir
        botaotag() {
            this.btag = true;
            this.count++;
            if (this.count >= 2) {
                this.btag = false;
                this.count = 0;
            }
        },
        botaoadd() {
            this.badd = true
        }
    }
}
</script>
<style scoped>
.box {
    display: flex;
    justify-content: space-evenly;
}

.pesquisa {
    background-color: blue;
    color: white;
}
</style>