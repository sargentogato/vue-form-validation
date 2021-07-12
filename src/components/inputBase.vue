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
    passwordData: {
      type: [String, Number],
    },
  },
  data() {
    return {
      inputInfo: null,
      showAdviceMessage: false,
      message: "Debe rellenar este campo",
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
      if (this.id === "password") this.validatePassword();
      if (this.id === "repeatPassword") this.passwordRepeated();
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
        this.inputIsOk(this.id, this.inputInfo);
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
        this.inputIsOk(this.id);
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
        this.inputIsOk(this.id);
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
        this.inputIsOk(this.id, this.inputInfo);
      }
    },
    validatePassword() {
      const nameMessage =
        "Requerido mínimo de 6 y máximo de 13 dígitos que contenga mayúsculas, minúsculas y números";
      const regPassword =
        /(?=^.{6,}$)((?=.*\d)|(?=.*\W+))(?![.\n])(?=.*[A-Z])(?=.*[a-z]).*$/;
      const isPasswordOk = regPassword.test(this.inputInfo);

      const nameSize = this.inputInfo.length;
      if (nameSize < 6 || nameSize > 13 || !isPasswordOk) {
        this.messageCreator(nameMessage, true);
        this.setErrorInput(this.id);
      } else {
        this.takeOffAdviceMessage();
      }

      if (!this.showAdviceMessage) {
        this.inputIsOk(this.id, this.inputInfo);
      }
    },
    passwordRepeated() {
      const passMessage = "La constraseña no coincide";

      if (this.inputInfo !== this.passwordData) {
        this.messageCreator(passMessage, true);
      } else {
        this.takeOffAdviceMessage();
      }

      if (!this.showAdviceMessage) {
        this.inputIsOk(this.id);
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
    inputIsOk(...value) {
      this.$emit("inputPass", value);
    },
    setErrorInput(idInput) {
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
