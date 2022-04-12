<template >
    <div>
        <Message :msg="msg" v-show="msg" />
    </div>
    <div>
        <form id="burger-form" @submit="createBurger">
            <div class="input-container">
                <label for="name">Nome do Cliente:</label>
                <input type="text" id="nome" name="name" v-model="nome" placeholder="Digite o seu nome">
            </div>
            <div class="input-container">
                <label for="pao">Escolha o Pão:</label>
                <select name="pao" id="pao" v-model="pao">
                    <option value="">Selecione o seu Pão</option>
                    <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
                       {{ pao.tipo }} 
                    </option>
                </select>
            </div>
            <div class="input-container">
                <label for="carne">Escolha a Carne:</label>
                <select name="carne" id="carne" v-model="carne">
                    <option value="">Selecione a Carne:</option>
                    <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo"> {{ carne.tipo }} </option>
                </select>
            </div>
            <div class="input-container">
                <label for="adicionais">Selecione os Adicionais:</label>
                <div v-for="adicional in adicionaisdata" :key="adicional.id" class="checkbox-container">
                    <input type="checkbox" name="adicionais" v-model="adicionais" :value="adicional.tipo">
                    <span> {{ adicional.tipo }} </span>
                </div>
            </div>
            <div class="input-container">
                <input type="submit" class="submit-btn" value="Criar meu Burguer">
            </div>
        </form>
    </div>
</template>
<script>
import Message from './Message.vue'

export default {
    name: 'BurgerForm',
    components: {
      Message
    },
    data() {
        return {
            paes: [{}],
            carnes: null,
            adicionaisdata: null,
            nome: null,
            pao: null,
            carne: null,
            adicionais: [],
            msg: null,
        }
    },
    methods: {
        async getIngredientes() {

            const req = await fetch("http://localhost:3000/box_e09a37a9fbc4972b9488/make_burguer");
            const data = await req.json();

            this.paes = data[0].ingredientes.paes
            this.carnes = data[0].ingredientes.carnes
           this.adicionaisdata = data[0].ingredientes.opcionais
        },
        async createBurger(e) {
          e.preventDefault();

          const data = {
            nome: this.nome,
            carne: this.carne,
            pao: this.pao,
            adicionais: Array.from(this.adicionais),
            status: "Solicitado"
          }
          const dataJson = JSON.stringify(data);

          const req = await fetch("http://localhost:3000/box_e09a37a9fbc4972b9488/burgers", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: dataJson
          });

          const res = await req.json();

          this.msg = "Pedido Realizado com Sucesso!"

          setTimeout(() => this.msg = "", 3000)

          this.nome = "";
          this.carne = "";
          this.pao = "";
          this.adicionais = [];
        }
    },
    mounted() {
      this.getIngredientes();
    },
}
</script>
<style>
    #burger-form {
    max-width: 400px;
    margin: 0 auto;
  }
  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }
  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
  }
  input, select {
    padding: 5px 10px;
    width: 300px;
  }
  #adicionais-container {
    flex-direction: row;
    flex-wrap: wrap;
  }
  #adicionais-title {
    width: 100%;
  }
  .checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
  }
  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }
  .checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
  }
  .submit-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  .submit-btn:hover {
    background-color: transparent;
    color: #222;
  }
    
</style>