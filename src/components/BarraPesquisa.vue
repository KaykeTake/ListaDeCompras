<template>
    <div>
        <textarea
            @keyup="search"
            v-model="item"
            class="barra"
            placeholder="Add Item"
        ></textarea>
    </div>
</template>
<script>

export default{
    name: "BarraPesquisa",
    components: { 
    },
    props:{
        clicks: Boolean,
    },
    watch: {
        clicks(){
            if(this.clicks === true){
                this.$emit('receber', this.item)
                this.limpar()
            }
        },
        apagando(){
            this.limpar()
        }
    },
    data(){
        return{
            item: ''
        }
    },
    methods: {
        search(){
            this.$emit('recebendopesquisa', this.item)
        },
        limpar(){
            this.item = ''
        }
    }
}
</script>
<style scoped>
.barra{
    resize: none;
}
</style>