<template>
  <div id="app">
    <div class="card">
      <form @submit.prevent="submitForm" class="form-container">
        <div class="form-group">
          <label for="numero">Digite um número:</label>
          <input
            type="number"
            id="numero"
            name="numero"
            v-model="formData.numero"
            required
          />
          <button type="submit" class="btn-submit">Traduzir</button>
        </div>
      </form>
      <div v-if="formData.extenso != ''" class="result">
        {{ formData.extenso }}
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      formData: {
        numero: null,
        extenso: "",
      },
    };
  },
  methods: {
    async submitForm() {
      if (this.formData.numero < 0 || this.formData.numero > 999) {
        alert("Por favor, digite um número entre 0 e 999.");
        return;
      }
      await axios
        .post("http://localhost:3000/tradutor", this.formData)
        .then((response) => {
          this.formData.extenso = response.data.extenso;
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  width: 500px;
  height: 400px;
  background: rgba(3, 67, 135, 0.748);
  border-radius: 8px;
}
.form-container {
  height: 50%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}
.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  font-weight: bold;
  font-size: 32px;
  margin-bottom: 20px;
}

input[type="text"],
input[type="number"] {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
.btn-submit {
  background-color: #4caf50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 20px;
  width: 100%;
  font-size: 18px;
}

.btn-submit:hover {
  background-color: #45a049;
}
.result {
  text-align: center;
  font-size: 32px;
  text-transform: capitalize;
  padding: 16px;
  background: rgba(18, 74, 117, 0.666);
}
</style>