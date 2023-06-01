<template>
  <h1>Componente de mensagem</h1>
  <div>
    <form id="lunch-form">
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
        <label for="pao">Escolha um tipo de pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option value="">Selecione o seu pão</option>
          <option value="">Integral</option>
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha um tipo de carne:</label>
        <select name="carne" id="carne" v-model="carne">
          <option value="">Selecione o seu tipo de carne</option>
          <option value="maminha">Maminha</option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
        <div class="checkbox-container">
          <input type="checkbox" name="opcionais" v-model="opcionais" value="salame">
          <span>Salame</span>
        </div>
        <div class="checkbox-container">
          <input type="checkbox" name="opcionais" v-model="opcionais" value="salame">
          <span>Salame</span>
        </div>
        <div class="checkbox-container">
          <input type="checkbox" name="opcionais" v-model="opcionais" value="salame">
          <span>Salame</span>
        </div>
      </div>
      <div class="input-container">
        <input type="text" class="submit-btn" value="Criar minha refeição!">
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "LunchForm,"
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
  border: 2px solid #588157;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: .5s
}

.submit-btn:hover {
  background-color: transparent;
}

</style>