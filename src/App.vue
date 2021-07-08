<template>
  <div id="app">
    <h1>Title</h1>
    <form action="">
      <input-base
        :inputType="type.text"
        :id="idInput.name"
        :placeholderInfo="placeHolderMessage.name"
        :clearInputs="clean"
        v-on:inputPass="inputManager"
      />
      <input-base
        :inputType="type.number"
        :id="idInput.tel"
        :placeholderInfo="placeHolderMessage.tel"
        :clearInputs="clean"
        v-on:inputPass="inputManager"
      />
      <input-base
        :inputType="type.number"
        :id="idInput.postalCode"
        :placeholderInfo="placeHolderMessage.postalCode"
        :clearInputs="clean"
        v-on:inputPass="inputManager"
      />
      <button
        type="submit"
        :disabled="inputOkChecker"
        v-on:click.prevent="sendInfo"
      >
        Enviar
      </button>
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
      clean: false,
      isDisable: true,
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
      this.errorManager();
    },
    inputManager(event) {
      this.inputsOk.push(event);
      this.okChecker();
    },
    okChecker() {
      const nOfInputsOk = this.inputsOk.length;
      if (nOfInputsOk >= 3) {
        this.isDisable = false;
      }
    },
    errorManager() {
      const inputsPass = this.inputsOk.length;
      const nInputs = this.nOfInputs().length;
      let errorsInput = this.checkErrors();

      if (!this.inputsOk.length) return;
      if (!errorsInput.length && inputsPass === nInputs) {
        this.cleanInput();
      }
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
      this.inputsOk = [];
      this.isDisable = true;
      if (!this.clean) return (this.clean = true);
      if (this.clean) return (this.clean = false);
    },
  },
  computed: {
    inputOkChecker() {
      return this.isDisable ? true : false;
    },
  },
};
</script>

NameInput
<style></style>
