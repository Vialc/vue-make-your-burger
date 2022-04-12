<template>
    <div id="burger-table">
        <Message :msg="msg" v-show="msg" />
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#: </div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Adicionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id" >
                <div class="order-number"> {{ burger.id }} </div>
                <div> {{ burger.nome }} </div>
                <div> {{ burger.pao }} </div>
                <div> {{ burger.carne }} </div>
                <div>
                    <ul>
                        <li v-for="(adicional, index) in burger.adicionais" :key="index">{{ adicional }}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" class="status" @change="updatedBurger($event, burger.nome)">
                        <option value="">Selecione</option>
                        <option v-for="acao in status" :key="acao.id" :value="acao.tipo" :selected="burger.status == acao.tipo">
                            {{ acao.tipo }}
                        </option>
                    </select>
                    <button class="delete-btn" @click="deleteBurger(burger._id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Message from './Message.vue'


export default {
    name: 'Dashboard',
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            nome: null,
            pao: null,
            carne: null,
            adicionais: [],
            msg: null
        }
    },
    methods: {
        async getPedidos() {
            const req = await fetch("http://localhost:3000/box_e09a37a9fbc4972b9488/burgers");

            const data = await req.json();

            this.burgers = data;

            this.getStatus()
        },
        async getStatus() {
            const req = await fetch("http://localhost:3000/box_e09a37a9fbc4972b9488/status")

            const data = await req.json();

            this.status = data;
        },
        async deleteBurger(_id) {
            const req = await fetch(`http://localhost:3000/box_e09a37a9fbc4972b9488/${_id}`, {
                method: "DELETE"
            });

            const res = await req.json();

            this.msg = "Pedido Cancelado com Sucesso!"

          setTimeout(() => this.msg = "", 3000)

            this.getPedidos();
        },
        async updatedBurger(event, nome) {
            const option = event.target.value;

            const take = await fetch(`http://localhost:3000/box_e09a37a9fbc4972b9488?q=nome:${nome}`, {
                method: "GET"
            });
            const burger = await take.json()
            
            const data = {
                _id: burger[0]._id,
                id: burger[0].id,
                nome: burger[0].nome,
                carne: burger[0].carne,
                pao: burger[0].pao,
                adicionais: Array.from(burger[0].adicionais),
                status: option
            }

            const dataJson = JSON.stringify(data)
            const req = await fetch(`http://localhost:3000/box_e09a37a9fbc4972b9488/${burger[0]._id}`, {
                method: "PUT",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            const res = await req.json();
            console.log(res)

            this.msg = `O pedido Nº ${res.id} foi atualizado para ${res.status}!`

          setTimeout(() => this.msg = "", 3000)
        },
    },
    mounted() {
        this.getPedidos();
    },
    components: {
        Message
    }
}
</script>
<style scoped>
    #burger-table {
        max-width: 1200px;
        margin: 0 auto;
    }

    #burger-table-heading,
    #burger-table-rows,
    .burger-table-row {
        display: flex;
        flex-wrap: wrap;
    }

    #burger-table-heading {
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #burger-table-heading div,
    .burger-table-row div {
        width: 19%;
    }

    .burger-table-row {
        width: 100%;
        padding: 12px;
        border-bottom: 1px solid #333;
    }

    #burger-table-heading .order-id,
    .burger-table-row .order-number {
        width: 5%;
    }

    select {
        padding: 12px 6px;
        margin-right: 12px;
        width: 50%;
    }

    .delete-btn {
        background-color: #222;
        color: #FCBA03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .delete-btn:hover {
        background-color: transparent;
        color: #222;
    }
</style>