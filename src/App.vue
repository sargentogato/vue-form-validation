<template>
  <div id="app" class="container">
    <div class="title d-flex justify-content-center">
      <h1 class="mt-3 mb-0 text-center text-light">{{ title }}</h1>
    </div>

    <div class="inputs d-flex justify-content-center">
      <form action="" class="d-flex flex-column">
        <input-base
          :inputType="type.text"
          :id="idInput.name"
          :placeholderInfo="placeHolderMessage.name"
          :clearInputs="clean"
          :passwordData="password"
          v-on:inputPass="inputManager"
          v-on:setErrorInput="setError"
        />
        <input-base
          :inputType="type.number"
          :id="idInput.tel"
          :placeholderInfo="placeHolderMessage.tel"
          :clearInputs="clean"
          :passwordData="password"
          v-on:inputPass="inputManager"
          v-on:setErrorInput="setError"
        />
        <input-base
          :inputType="type.number"
          :id="idInput.postalCode"
          :placeholderInfo="placeHolderMessage.postalCode"
          :clearInputs="clean"
          :passwordData="password"
          v-on:inputPass="inputManager"
          v-on:setErrorInput="setError"
        />
        <input-base
          :inputType="type.email"
          :id="idInput.email"
          :placeholderInfo="placeHolderMessage.email"
          :clearInputs="clean"
          :passwordData="password"
          v-on:inputPass="inputManager"
          v-on:setErrorInput="setError"
        />
        <input-base
          :inputType="type.password"
          :id="idInput.password.pass"
          :placeholderInfo="placeHolderMessage.password.pass"
          :clearInputs="clean"
          :passwordData="password"
          v-on:inputPass="inputManager"
          v-on:setErrorInput="setError"
        />
        <input-base
          :inputType="type.password"
          :id="idInput.password.repeatPassword"
          :placeholderInfo="placeHolderMessage.password.repeatPassword"
          :clearInputs="clean"
          :passwordData="password"
          v-on:inputPass="inputManager"
          v-on:setErrorInput="setError"
        />
        <button
          class="btn btn-primary"
          type="submit"
          :disabled="inputsOkChecker"
          v-on:click.prevent="sendInfo"
        >
          Enviar
        </button>
      </form>
    </div>
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
      title: "IT ACADEMY FORM",
      inputsOk: [],
      user: {},
      clean: false,
      isDisable: true,
      password: "",
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
        email: "email",
        password: {
          pass: "password",
          repeatPassword: "repeatPassword",
        },
      },
      placeHolderMessage: {
        name: "Nombre",
        tel: "Teléfono",
        postalCode: "Código Postal",
        email: "Correo eléctronico",
        password: {
          pass: "Contraseña",
          repeatPassword: "Repita Contraseña",
        },
      },
    };
  },
  methods: {
    sendInfo() {
      this.errorManager();
    },
    inputManager(event) {
      if (event[0] === "name" && !this.inputsOk.includes(event[0])) {
        this.user["name"] = event[1];
      }

      if (event[0] === "password") {
        this.password = event[1];
      }

      if (!this.inputsOk.includes(event[0])) {
        this.inputsOk.push(event[0]);
      }

      this.okChecker();
    },
    setError(event) {
      if (this.inputsOk.includes(event)) {
        const indexToTakeOff = this.inputsOk.indexOf(event);
        this.inputsOk.splice(indexToTakeOff, 1);
        this.isDisable = true;
      }
    },
    okChecker() {
      const nOfInputsOk = this.inputsOk.length;
      const nInputs = this.nOfInputs().length;

      if (nOfInputsOk === nInputs) {
        this.isDisable = false;
      }
    },
    errorManager() {
      const inputsPass = this.inputsOk.length;
      const nInputs = this.nOfInputs().length;
      let errorsInput = this.checkErrors();

      if (!errorsInput.length && inputsPass === nInputs) {
        this.cleanInput();
      }
    },
    nOfInputs() {
      const formElements = document.forms[0];
      return formElements.querySelectorAll("input");
    },
    checkErrors() {
      const formInpunts = document.forms[0];
      return formInpunts.querySelectorAll("p");
    },
    cleanInput() {
      alert(`${this.user.name} te has registrado correctamente`);
      this.reseteValues();
      if (!this.clean) return (this.clean = true);
      if (this.clean) return (this.clean = false);
    },
    reseteValues() {
      this.inputsOk = [];
      this.isDisable = true;
      this.user = {};
    },
  },
  computed: {
    inputsOkChecker() {
      return this.isDisable ? true : false;
    },
  },
};
</script>

NameInput
<style scoped>
.container {
  height: 100vh;
}

.title {
  height: 20%;
}

.inputs {
  height: 80%;
}

form {
  flex-basis: 30%;
  gap: 0.8em;
  margin-block-start: 2em;
}
</style>
