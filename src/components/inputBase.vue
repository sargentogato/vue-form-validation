<template>
  <div>
    <input
      :id="id"
      :type="inputType"
      :placeholder="placeholderInfo"
      v-model="inputInfo"
      class="form-control"
    />
    <p v-if="showAdviceMessage">{{ message }}</p>
  </div>
</template>
<script>
export default {
  name: "inputBase",
  props: {
    inputType: {
      type: [String, Number],
    },
    id: {
      type: String,
    },
    placeholderInfo: {
      type: String,
    },
    checkInputs: {
      type: Boolean,
    },
    cleanInputs: {
      type: Boolean,
    },
  },
  data() {
    return {
      inputInfo: null,
      showAdviceMessage: false,
      message: "Debe rellenar este campo",
      inputOk: [],
      clearInput: this.cleanInputs,
    };
  },
  methods: {
    validationManager() {
      if (!this.inputInfo) return (this.showAdviceMessage = true);
      console.log("PROP:", this.cleanInputs, "/", "DATA:", this.clearInput);
      this.checkDataInput();
    },
    checkDataInput() {
      if (this.id === "name") this.validateName();
      if (this.id === "phone") this.validateNumber();
      if (this.id === "postalCode") this.validatePostalCode();
    },
    validateName() {
      const nameMessage =
        "El nombre debe tenere minímo 6 carácteres y máximo 13";

      const nameSize = this.inputInfo.length;
      if (nameSize < 6 || nameSize > 13)
        return this.messageCreator(nameMessage, true);

      if (this.inputOk.includes("name")) return;

      this.showAdviceMessage = false;
      if (!this.showAdviceMessage) {
        this.inputOk.push(this.id);
        this.okManager();
        console.log(this.inputOk);
      }
    },
    validateNumber() {
      const numberMessage = "Este campo sólo puede contener números";

      const regex = /^[0-9]*$/;
      if (!regex.test(this.inputInfo)) {
        return this.messageCreator(numberMessage, true);
      }
      this.takeOffAdiviceMessage();
      if (this.inputOk.includes("phone")) return;

      // this.showAdviceMessage = false;
      if (!this.showAdviceMessage) {
        this.inputOk.push(this.id);
        this.okManager();
        console.log("Validate Number:", this.inputOk);
      }
    },
    validatePostalCode() {
      const numberMessage = "Escriba un código postal correcto";

      if (this.inputInfo.length < 5) {
        return this.messageCreator(numberMessage, true);
      }
      this.takeOffAdiviceMessage();
      if (this.inputOk.includes("postalCode")) return;

      // this.showAdviceMessage = false;
      if (!this.showAdviceMessage) {
        this.inputOk.push(this.id);
        this.okManager();
        console.log("Validate Number:", this.inputOk);
      }
    },
    takeOffAdiviceMessage() {
      this.showAdviceMessage = false;
    },
    messageCreator(...message) {
      let [messageToShow, showOrder] = message;
      this.message = messageToShow;
      this.showAdviceMessage = showOrder;
    },
    okManager() {
      this.$emit("inputPass", this.inputOk[0]);
    },
  },
  watch: {
    checkInputs() {
      console.log("boton presionado");
      if (this.checkInputs || !this.inputInfo) this.validationManager();
    },
    inputInfo() {
      this.validationManager();
    },
  },
};
</script>

<style lang="scss" scoped></style>
