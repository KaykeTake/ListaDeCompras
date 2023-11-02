<template>
    <input type="button" @click="enviar" :value= 'name' >
</template>

<script>
export default{
    name: 'BotaoComponent',
    props:{
        //recebe o nome que será dado a o botão
        name: String
    },
    methods:{
        enviar(){
            //envia uma confirmação que o botão foi clicado para executar as ações necessárias 
            this.$emit('receber')
        }
    }
}
</script>
<style scoped>
</style>