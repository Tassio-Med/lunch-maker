<template>
  <div id="lunch-table">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="lunch-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Massa:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="lunch-table-rows">
      <div class="lunch-table-row" v-for="lunch in lunches" :key="lunch.id">
        <div class="order-number">{{ lunch.id }}</div>
        <div>{{ lunch.nome }}</div>
        <div>{{ lunch.massa }}</div>
        <div>{{ lunch.carne }}</div>
        <div>
          <ul>
            <li v-for="(opcional, index) in lunch.opcionais" :key="index">
              {{ opcional }}
            </li>
          </ul>
        </div>
        <div>
          <select name="status" class="status" @change="updatedLunch($event, lunch.id)">
            <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="lunch.status == s.tipo">
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteLunch(lunch.id)">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import Message from './Message.vue';

export default {
  name: "Dashboard",

  components: {
    Message
  },
  data(){
    return {
      lunches: null,
      lunch_id: null,
      status: [],
      msg: null
    }
  },
  methods: {
    async getPedidos(){

      const req = await fetch("http://localhost:3000/lunches");

      const data = await req.json();

      this.lunches = data;

      this.getStatus();
    },

    async getStatus() {
      
      const req = await fetch("http://localhost:3000/status");

      const data = await req.json();

      this.status = data;

    },

    async deleteLunch(id) {

      const req = await fetch(`http://localhost:3000/lunches/${id}`, {
        method: "DELETE"
      });

      const res = await req.json();

      this.msg = `Pedido removido com sucesso!`;

      setTimeout(() => this.msg = "", 3000)

      this.getPedidos();
    },

    async updatedLunch(event, id) {

      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option });

       const req = await fetch(`http://localhost:3000/lunches/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "applicaton/json" },
        body: dataJson
      });

      const res = await req.json();

      // console.log(res)

      this.msg = `O pedido Nª ${res.id} foi atualizado para ${res.status}!`;

      setTimeout(() => this.msg = "", 3000)

    }

  },
  mounted(){
    this.getPedidos();
  }
}
</script>

<style scoped>
  #lunch-table{
    max-width: 1200px;
    margin: 0 auto;
  }

  #lunch-table-heading,
  #lunch-table-rows,
  .lunch-table-row {
    display: flex;
    flex-wrap: wrap;
  }

  #lunch-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  #lunch-table-heading div,
  .lunch-table-row div {
    width: 19%;
  }

  .lunch-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #ccc;
  }

  #lunch-table-heading .order-id,
  .lunch-table-row .order-number {
    width: 5%;
  }

  select {
    padding: 12px 6px;
    margin-right: 12px;
  }

  .delete-btn {
    background-color: #ff0000c8;
    border-radius: 10px;
    color:#fff;
    font-weight: bold;
    border: none;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {
    background-color: #ff0000;
  }

</style>

