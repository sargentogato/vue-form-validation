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
      // inputOk: [],
    };
  },
  methods: {
    validationManager() {
      if (!this.inputInfo) {
        this.showAdviceMessage = true;
        this.setErrorInput(this.id);
        return;
      }
      this.checkDataInput();
    },
    checkDataInput() {
      if (this.id === "name") this.validateName();
      if (this.id === "phone") this.validatePhoneNumber();
      if (this.id === "postalCode") this.validatePostalCode();
      if (this.id === "email") this.validateEmail();
    },
    validateName() {
      const nameMessage =
        "El nombre no puede contener números, debe tenere minímo 6 carácteres y máximo 13,  ";
      const isThereAnyNumber = this.areThereAnyNumber(this.id, this.inputInfo);
      const nameSize = this.inputInfo.length;
      if (nameSize < 6 || nameSize > 13 || isThereAnyNumber) {
        this.messageCreator(nameMessage, true);
        this.setErrorInput(this.id);
      } else {
        this.takeOffAdviceMessage();
      }

      if (!this.showAdviceMessage) {
        this.okManager(this.id, this.inputInfo);
      }
    },
    validatePhoneNumber() {
      const numberMessage =
        "Introduzca un número de teléfono valido. Debe de tener como minímo 9 digitos";

      const isNotANumber = this.areThereAnyNumber(this.inputInfo);

      if (!isNotANumber || this.inputInfo.length < 9) {
        this.messageCreator(numberMessage, true);
        this.setErrorInput(this.id);
      } else {
        this.takeOffAdviceMessage();
      }

      if (!this.showAdviceMessage) {
        this.okManager(this.id);
      }
    },
    validatePostalCode() {
      const postalCodeMessage =
        "El código postal no puede contener letras y debe tener minímo 5 números";

      const isNotANumber = this.areThereAnyNumber(this.inputInfo);

      if (this.inputInfo.length < 5 || !isNotANumber) {
        this.messageCreator(postalCodeMessage, true);
        this.setErrorInput(this.id);
      } else {
        this.takeOffAdviceMessage();
      }

      if (!this.showAdviceMessage && this.inputInfo >= 5) {
        this.okManager(this.id);
      }
    },
    validateEmail() {
      const emailMessage = "El email no es valido";

      const regexEmail =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;

      if (!regexEmail.test(this.inputInfo)) {
        this.messageCreator(emailMessage, true);
        this.setErrorInput(this.id);
      } else {
        this.takeOffAdviceMessage();
      }

      if (!this.showAdviceMessage) {
        this.okManager(this.id, this.inputInfo);
      }
    },
    areThereAnyNumber(...value) {
      const regexOnlyNumbers = /^[0-9]*$/;
      const regexNumberInString = /\d/;
      if (value[0] === "name") {
        return regexNumberInString.test(value[1]);
      } else {
        return regexOnlyNumbers.test(value);
      }
    },
    takeOffAdviceMessage() {
      this.showAdviceMessage = false;
    },
    messageCreator(...message) {
      let [messageToShow, showOrder] = message;
      this.message = messageToShow;
      this.showAdviceMessage = showOrder;
    },
    okManager(...value) {
      // console.log("OK okManager", value);
      this.$emit("inputPass", value);
    },
    setErrorInput(idInput) {
      console.log("Evento");
      this.$emit("setErrorInput", idInput);
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
