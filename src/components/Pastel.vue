<template>
    <div>
        <Message :msg="msg" v-show="msg"/>
        <div>
            <form id="pastel-form" method="POST" @submit="createPastel">
                <div class="input-container">
                    <label for="nome">Nome do Cliente</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite seu nome">
                </div>
        
                <div class="input-container">
                    <label for="tamanho">Selecione o tamanho:</label>
                    <select name="tamanho" id="tamanho" v-model="tamanho">
                    <option value="">Selecione o tamanho</option>
                    <option v-for="tamanho in tamanhos" :key="tamanho.id" :value="tamanho.tipo">
                        {{tamanho.tipo}}
                        </option>
                    </select>
                </div>
                
                <div class="input-container">
                    <label for="Recheio">Selecione o Recheio:</label>
                    <select name="recheio" id="recheio" v-model="recheio">
                    <option value="">Selecione o recheio</option>
                    <option v-for="recheio in recheios" :key="recheio.id" :value="recheio.tipo">
                        {{recheio.tipo}}
                    </option>
                    </select>
                </div>

                <div id="opcionais-container" class="input-container">
                    <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
                    <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{opcional.tipo}}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Crie meu Pastel!">

                </div>
            </form>
        </div>
    </div>   
</template>

<script>
import Message from './Message.vue'

export default{
    name: "Pastel",
    data(){
        return{

            tamanhos: null,
            recheios: null,
            opcionaisdata: null,
            nome: null,
            tamanho:null,
            recheio: null,
            opcionais: [],
            msg: null


        }
    },
    methods: {
        async getIngredientes(){

            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();
            
            this.tamanhos = data.tamanhos;
            this.recheios = data.recheios;
            this.opcionaisdata = data.opcionais

        },


        async createPastel(e) {

            e.preventDefault();

            const data = {
                nome: this.nome,
                tamanho: this.tamanho,
                recheio: this.recheio,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado"
            }

            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/pasteis",{
                method: "POST",
                headers: { "Content-Type" : "application/json"},
                body: dataJson
            });

            const res = await req.json();


            //mensagem do sistema
            this.msg = `Pedido Nº ${res.id} realizado com Sucesso!`;

            setTimeout(() => this.msg = "", 3000);

            //limpando os campos
            this.nome = "";
            this.tamanho = "";
            this.recheio = "";
            this.opcionais = [];



        }
    },
    mounted(){
        this.getIngredientes()
    },
    components: {
        Message
    }
}

</script>

<style scoped>

#pastel-form{
    max-width: 400px;
    margin: 0 auto;
   }

.input-container{
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label{
    font-weight: bold;
    margin-bottom: 15px;
    color: black;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
}

input, select{
    padding: 5px 10px;
    width: 300px;
}

#opcionais-container{
    flex-direction: row;
    flex-wrap: wrap;
}

#opcionais-title{
    width: 100%;
}

.checkbox-container{
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input{
    width: auto;
}

.checkbox-container span{
    margin-left: 6px;
    font-weight: bold;
}

.submit-btn{
    background-color: black;
    color:#fcba03;
    font-weight: bold;
    border:1px solid #fcba03;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
}

</style>