<template>
  <Message :msg="msg" v-show="msg" />
  <div>
    <form id="lunch-form" @submit="createLunch">
      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input 
          type="text"
          id="nome"
          name="nome"
          v-model="nome"
          placeholder="Digite o seu nome"
        >
      </div>
      <div class="input-container">
        <label for="massa">Escolha um tipo de massa:</label>
        <select name="massa" id="massa" v-model="massa">
          <option value="">Selecione um tipo de massa</option>
          <option v-for="massa in massas" :key="massa.id" :value="massa.tipo">
            {{ massa.tipo }}
          </option>
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha um tipo de carne:</label>
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione um tipo de carne</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
            {{ carne.tipo }}
          </option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
        <div
          class="checkbox-container"
          v-for="opcional in opcionaisdata"
          :key="opcional.id"
        >
          <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>
      <div class="input-container">
        <input type="submit" class="submit-btn" value="Criar minha refeição!">
      </div>
    </form>
  </div>
</template>

<script>

import Message from './Message.vue'

export default {
  name: "LunchForm",

  data(){
    return {
      massas: null,
      carnes: null,
      opcionaisdata: null,
      nome: null,
      carne: null,
      opcionais: [],
      status: "Solicitado",
      msg: null
    }
  },

  methods: {
    async getIngredientes() {

      const req = await fetch('http://localhost:3000/ingredientes');
      const data = await req.json();

      this.massas = data.massas;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;

    },
    async createLunch(e) {
      
      e.preventDefault()

      const data = {
        nome: this.nome,
        carne: this.carne,
        massa: this.massa,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado"
      }

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/lunches", {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: dataJson
      });

      const res = await req.json();

      this.msg=`Pedido Nª ${res.id} realizado com sucesso!`;

      setTimeout(() => this.msg = "", 3000);

      this.nome = "";
      this.carne = "";
      this.massa = "";
      this.opcionais = "";

    }
  },
  mounted() {
    this.getIngredientes()
  },

  components: {
    Message
  }
}
</script>

<style scoped>
#lunch-form {
  max-width: 400px;
  margin: 0 auto;
  background-color: #344e41;
  padding: 20px 20px;
  border-radius: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #fff;
  padding: 5px 10px;
  border-left: 4px solid #588157;
}

input, select {
  padding: 5px 10px;
  width: 300px;
}

#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
  color: #fff;
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
  text-align: center;
  background-color: #588157;
  border-radius: 10px;
  color: #fff;
  font-weight: bold;
  border: none;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: .5s
}

.submit-btn:hover {
  background-color: #66aa66;
}

</style>