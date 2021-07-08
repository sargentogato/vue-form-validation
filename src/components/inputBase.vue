<template>
  <div>
    <input
      :id="id"
      :type="inputType"
      :placeholder="placeholderInfo"
      v-model="inputInfo"
      class="form-control"
      @blur="validationManager"
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
    clearInputs: {
      type: Boolean,
    },
  },
  data() {
    return {
      inputInfo: null,
      showAdviceMessage: false,
      message: "Debe rellenar este campo",
      inputOk: [],
    };
  },
  methods: {
    validationManager() {
      if (!this.inputInfo) return (this.showAdviceMessage = true);
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
      if (nameSize < 6 || nameSize > 13) {
        return this.messageCreator(nameMessage, true);
      }

      this.takeOffAdiviceMessage();
      if (!this.showAdviceMessage) {
        this.okManager(this.id);
      }
    },
    validateNumber() {
      const numberMessage = "Este campo sólo puede contener números";

      const regex = /^[0-9]*$/;
      if (!regex.test(this.inputInfo)) {
        return this.messageCreator(numberMessage, true);
      }

      this.takeOffAdiviceMessage();

      this.showAdviceMessage = false;
      if (!this.showAdviceMessage) {
        this.okManager(this.id);
      }
    },
    validatePostalCode() {
      const numberMessage = "Escriba un código postal correcto";

      if (this.inputInfo.length < 5) {
        this.messageCreator(numberMessage, true);
        return;
      }

      this.takeOffAdiviceMessage();

      if (!this.showAdviceMessage) {
        this.inputOk.push(this.id);
        this.okManager(this.id);
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
    okManager(value) {
      this.$emit("inputPass", value);
    },
  },
  watch: {
    clearInputs() {
      this.inputInfo = "";
    },
  },
};
</script>

<style lang="scss" scoped></style>
