<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <div>
      <form id="burguer-form" @submit="createBurger">
        <div class="input-container">
          <label for="name">Nome do Cliente: </label>
          <input
            type="text"
            id="nome"
            v-model="name"
            placeholder="Digite o nome do Cliente"
          />
        </div>
        <div class="input-container">
          <label for="bread">Selecione o Pão: </label>
          <select name="bread" id="bread" v-model="bread">
            <option value="">Selecione o tipo de pão</option>
            <option v-for="bread in paes" :key="bread.id" :value="bread.tipo">
              {{ bread.tipo }}
            </option>
          </select>
        </div>
        <div class="input-container">
          <label for="meat">Selecione a Carne: </label>
          <select name="meat" id="meat" v-model="meat">
            <option value="">Selecione o tipo de carne</option>
            <option v-for="meat in carnes" :key="meat.id" :value="meat.tipo">
              {{ meat.tipo }}
            </option>
          </select>
        </div>
        <div id="optional-container" class="input-container">
          <label for="optional">Selecione os opcionais: </label>
          <div
            class="checkbox-container"
            v-for="optionals in opcionais"
            :key="optionals.id"
          >
            <input
              type="checkbox"
              id="optional-title"
              name="optional"
              v-model="optional"
              :value="optionals.tipo"
            />
            <span>{{ optionals.tipo }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Confirmar Pedido" />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";
export default {
  name: "BurgerForm",
  data() {
    return {
      paes: null,
      carnes: null,
      opcionais: null,
      name: null,
      bread: null,
      meat: null,
      optional: [],
      msg: null,
    };
  },
  methods: {
    async getIngredients() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();

      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionais = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault();
      if(this.name && this.bread && this.meat) {
        const data = {
        name: this.name,
        bread: this.bread,
        meat: this.meat,
        optional: Array.from(this.optional),
        status: "Solicitado",
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.msg = `Pedido Nº ${res.id} Realizado com sucesso!`;

      //limpar msg
      setTimeout(() => (this.msg = ""), 2000);

      //limpar os campos
      this.name = "";
      this.bread = "";
      this.meat = "";
      this.optional = "";
      }
    },
  },
  mounted() {
    this.getIngredients();
  },
  components: {
    Message,
  },
};
</script>

<style scoped>
#burguer-form {
  max-width: 400px;
  margin: 0 auto;
  padding: 40px;
  box-shadow: 5px 1px 10px rgba(0, 0, 0, 0.3);
  border-radius: 8px;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
  border: none;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#optional-container {
  flex-direction: row;
  flex-wrap: wrap;
}
#optional-title {
  widows: 100%;
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
  margin-left: 30px;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;

  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  border-radius: 8px;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: #239a35;
  color: #222;
  font-size: 20px;
}
</style>