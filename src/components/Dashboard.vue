<template>
  <div class="burger-table">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div class="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div class="status">Status:</div>
      </div>
      <div class="burger-table-rows">
        <div
          class="burger-table-row"
          v-for="burger in burgers" :key="burger.id"
        >
          <div class="order-number">{{ burger.id }}</div>
          <div>{{ burger.name }}</div>
          <div>{{ burger.bread }}</div>
          <div>{{ burger.meat }}</div>
          <div>
            <ul>
              <li v-for="(option, index) in burger.optional" :key="index">
                {{ option }}
              </li>
            </ul>
          </div>
          <div class="status-and-buttom-field">
            <select name="status" id="status" @change="updatedBurger($event,burger.id)">
              <option value="">Selecione</option>
              <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">{{ s.tipo }}</option>
            </select>
            <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar Pedido</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "Dashboard",
  components:{
    Message
  },
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      msg: null
    };
  },
  methods: {
    async getRequests() {
      const req = await fetch("http://localhost:3000/burgers");

      const data = await req.json();

      this.burgers = data;

      this.getStatus();
    },

    async getStatus() {
        const req = await fetch("http://localhost:3000/status")

        const data = await req.json();

        this.status = data;
    },

    async deleteBurger(id) {

      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method:"DELETE"
      })

      const res = await req.json();

      this.msg = `Pedido Nº ${res.id} foi removido com sucesso!`;

      
      setTimeout(() => (this.msg = ""), 2000);

      this.getRequests();

    },

    async updatedBurger(event, id) {
      
      const option = event.target.value;

      const dataJson = JSON.stringify({status: option});

      const req = await fetch(`http://localhost:3000/burgers/${id}`,{
        method:"PATCH",
        headers: {"Content-Type": "application/json"},
        body:dataJson
      });
       
      const res = await req.json();
      
      this.msg = `Pedido Nº ${res.id} foi atualizado para ${res.status}!`;

      
      setTimeout(() => (this.msg = ""), 2000);

    }

  },

  mounted() {
    this.getRequests();
  },
};
</script>

<style scoped>
.burger-table {
  max-width: 1200px;
  margin: 0 auto;
}
.burger-table-heading,
.burger-table-rows .burger-table-row {
  display: flex;
  flex-wrap: wrap;
}
.burger-table-heading {
  font-weight: bold;
  padding: 12px;
}
.burger-table-heading div,
.burger-table-row div {
  width: 19%;
}

.burger-table-row {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}

.status {
    width: 223px;
    display: flex;
    justify-content: center;
}

.status-and-buttom-field {
    width: 223px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

select {
  padding: 12px 6px;
  margin: 12px;
  border: 1px solid #222;
}

.delete-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
  width: 180px;
  border-radius: 8px;
}

.delete-btn:hover {
  background-color: rgb(255, 4, 4);
  color: #fff;
  font-size: 17px;
}
</style>