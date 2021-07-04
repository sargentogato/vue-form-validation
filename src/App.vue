<template>
  <div id="app">
    <h1>Title</h1>
    <form action="">
      <input-base
        :inputType="type.text"
        :id="idInput.name"
        :placeholderInfo="placeHolderMessage.name"
        :checkInputs="checkAllInputs"
        :cleanInputs="cleanAllInputs"
        v-on:inputPass="inputManager"
      />
      <input-base
        :inputType="type.number"
        :id="idInput.tel"
        :placeholderInfo="placeHolderMessage.tel"
        :checkInputs="checkAllInputs"
        :cleanInputs="cleanAllInputs"
        v-on:inputPass="inputManager"
      />
      <input-base
        :inputType="type.number"
        :id="idInput.postalCode"
        :placeholderInfo="placeHolderMessage.postalCode"
        :checkInputs="checkAllInputs"
        :cleanInputs="cleanAllInputs"
        v-on:inputPass="inputManager"
      />
      <button type="submit" v-on:click.prevent="sendInfo">Enviar</button>
    </form>
  </div>
</template>

<script>
import inputBase from "./components/inputBase.vue";
export default {
  name: "App",
  components: {
    inputBase,
  },
  data() {
    return {
      inputsOk: [],
      user: [],
      checkAllInputs: false,
      cleanAllInputs: false,
      type: {
        text: "text",
        number: "number",
        tel: "tel",
        email: "email",
        password: "password",
      },
      idInput: {
        name: "name",
        tel: "phone",
        postalCode: "postalCode",
        password: "password",
        repeatPassword: "repeatPassword",
      },
      placeHolderMessage: {
        name: "Nombre",
        tel: "Teléfono",
        postalCode: "Código Postal",
      },
    };
  },
  methods: {
    sendInfo() {
      this.checkAllInputs = true;
      this.errorManager();
    },
    inputManager(event) {
      console.log("Evento REcibido:", event);
      this.inputsOk.push(event);
      this.errorManager();
    },
    errorManager() {
      const inputsPass = this.inputsOk.length;
      const nInputs = this.nOfInputs().length;
      let errorsInput = this.checkErrors();

      if (!this.inputsOk.length) return;
      if (!errorsInput.length && inputsPass === nInputs) this.cleanInput();
    },
    nOfInputs() {
      const formElement = document.forms[0];
      return formElement.querySelectorAll("input");
    },
    checkErrors() {
      const formInpunts = document.forms[0];
      return formInpunts.querySelectorAll("p");
    },
    cleanInput() {
      // this.cleanAllInputs = true;
      alert();
    },
  },
};
</script>

NameInput
<style></style>
