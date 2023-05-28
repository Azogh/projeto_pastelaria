<template>
    <div id="pastel-table">
        <div>
            <div id="pastel-table-heading">
                <div class="order-id">#</div>
                <div>Cliente:</div>
                <div>Tamanho:</div>
                <div>Recheio:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="pastel-table-row">
            <div class="pastel-table-row" v-for="pastel in pasteis" :key="pastel.id">
                <div class="order-number">{{ pastel.id }}</div>
                <div>{{pastel.nome}}</div>
                <div>{{pastel.tamanho}}</div>
                <div>{{pastel.recheio}}</div>
                <div>
                    <ul>
                        <li v-for="(opcional, index) in pastel.opcionais" :key="index">
                            {{opcional}}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updatePastel($event, pastel.id)">
                        <option value=""> Selecione</option>
                        <option v-for="S in status" :key="S.id" :value="S.tipo" :selected="pastel.status == S.tipo">
                        {{ S.tipo }}
                    </option>
                    </select>
                    <button class="delete-btn" @click=deletePastel(pastel.id)>Concluido</button>
                </div>
            </div>
        </div>
    </div>    
</template>

<script>


export default {
  name: "Dashboard",
  data(){
    return{
        pasteis: null,
        pasteis_id: null,
        status: [],
    }
  },
  methods:{
    async getPedidos(){

        const req = await fetch("http://localhost:3000/pasteis");

        const data = await req.json();

        this.pasteis = data;

        //resgatar status
        this.getStatus();


    },
    async getStatus(){
        const req = await fetch("http://localhost:3000/status");

        const data = await req.json();
        
        this.status = data;


    },
    async deletePastel(id) {

        const req = await fetch(`http://localhost:3000/pasteis/${id}`, {
            method:"DELETE"
        });

        const res = await req.json();
        
        this.getPedidos();

        console.log(`Pedido nº ${id} concluido`)


    },

    async updatePastel(event, id) {

            const option = event.target.value;

            const dataJson = JSON.stringify({status : option});

            const req = await fetch(`http://localhost:3000/pasteis/${id}`,{
                method: "PATCH",
                headers: {"Content-Type" : "application/json"},
                body: dataJson
            });

            const res = await req.json();

            console.log(res);
        }
    
        },
        mounted(){
            this.getPedidos();
        }
    }

</script>

<style scoped>

    #pastel-table {
        max-width: 1200px;
        margin: 0 auto;
    }

    #pastel-table-heading,
    #pastel-table-rows,
    .pastel-table-row{
        display: flex;
        flex-wrap: wrap;
    }

    #pastel-table-heading{
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #fcba03;
    }

    #Messagepastel-table-heading div,
    .pastel-table-row div {
        width: 19%;
    }

    .pastel-table-row{
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #ccc;
    }

    #pastel-table-heading .order-id,
    .pastel-table-row .order-number{
        width: 5%;
    }

    select{
        padding: 12px 6px;
        margin-right: 12px;
    }

    .delete-btn{
        background-color: black;
        color: #fcba03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        margin: 0 auto;
        cursor: pointer;
    }


</style>