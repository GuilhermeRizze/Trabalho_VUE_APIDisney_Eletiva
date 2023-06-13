<template>           
        <input 
            type="text"
            placeholder="Pesquise um personagem"
            v-model="consulta"
            @input="onSearchChange"
            id="texto"
        >   
        <ul class="lista">
            <li v-for="(item, key) in itens" :key="key" class="persona">
                <div class="conteudo">
                    <a :href="item.imageUrl" target="_blank">
                        <img :src="item.imageUrl">
                    </a>
                    <h3>{{ item.name }}</h3>
                </div>
            </li>
        </ul>

</template>
<script>
    import api from '../services/api.js'

    export default {
        name: "CompDisney",
        components: {
            api
        },
        data() {
            return {
                consulta: "",
                itens: [],
                timeout: null
            }
        },
        mounted(){
            this.buscar()
        },
        methods: {
            buscar() {                
                let url = "&name=" + this.consulta
                api.get(url)
                    .then(retornar => {
                        if (Array.isArray(retornar.data.data)) {
                            this.itens = retornar.data.data;
                        } else {
                            this.itens = [retornar.data.data];
                        }
                    });
            },
            onSearchChange() {
                this.itens = [];
                clearTimeout(this.timeout);                
                this.timeout = setTimeout(() => {
                    this.buscar();
                }, 300);
            }
        }
    }
</script>
<style scoped>
    #texto{
        width: 96%;
        padding: 20px;
        margin: 20px;
        box-sizing: border-box;
        background: #899fff;
    }
    .lista
    {
        display: flex;
        flex-wrap: wrap;
    }
    .lista .persona
    {
        margin: 3px;
        border-radius: 10px;
        background: #0044ff;
        display: flex;
        align-items: center ;
        text-align: center;
        width: 200px;
        height: 200px;    }
    .lista .persona .conteudo a img
    {
        width: 200px;
        height: 140px;
    }
</style>
