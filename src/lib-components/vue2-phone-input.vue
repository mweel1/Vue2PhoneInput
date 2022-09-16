<template>
  <input
    type="text"
    v-bind="$attrs"
    v-model="phone"
    @keydown="keyDown"
    @paste="doPaste"
    maxlength="14"
  />
</template>

<script>
export default /*#__PURE__*/ {
  name: "Vue2PhoneInput", // vue component name
  data() {
    return { phone: "" };
  },
  props: { value: null },
  watch: {
    phone(newVal) {
      this.$emit("input", newVal);
    },
    value(newVal) {
      this.phone = newVal;
      if (this.phone) this.doFormat();
    },
  },
  created() {
    this.phone = this.value;

    if (this.phone) this.doFormat();
  },
  methods: {
    keyDown(e) {
      var curchr = this.phone.length;

      if ((e.key == "v" || e.key == "c" || e.key == "x") && e.ctrlKey) {
        return;
      }

      if (e.key == "Home" && e.shiftKey) {
        return;
      }

      if (
        !Number.isInteger(parseInt(e.key)) &&
        e.key != "Backspace" &&
        e.Key != "Delete" &&
        e.key != "ArrowLeft" &&
        e.key != "ArrowRight" &&
        e.key != "Delete" &&
        e.key != "Tab"
      ) {
        e.preventDefault();
        return;
      }

      if (curchr == 0 && e.key == "1") {
        e.preventDefault();
        return;
      }

      this.doFormat();

      if (e.which == 8) {
        console.log("here");

        if (this.phone.endsWith(" ")) {
          e.preventDefault();
          this.phone = this.phone.substring(0, this.phone.length - 3);
          return;
        }

        if (this.phone.endsWith("-")) {
          e.preventDefault();
          this.phone = this.phone.substring(0, this.phone.length - 2);
          return;
        }
      }
    },
    doPaste(e) {
      let paste = (e.clipboardData || window.clipboardData).getData("text");

      this.phone = paste;
      e.preventDefault();

      this.doFormat();
    },
    doFormat() {
      //Filter only numbers from the input
      let cleaned = ("" + this.phone).replace(/\D/g, "");

      if (cleaned.length == 0) {
        this.phone = "";
        return;
      }
      if (cleaned.length == 3) {
        this.phone = "(" + cleaned + ") ";
        return;
      } else if (cleaned.length == 6) {
        this.phone =
          "(" + cleaned.substr(0, 3) + ") " + cleaned.substr(3, 3) + "-";
        return;
      } else if (cleaned.length == 10) {
        this.phone =
          "(" +
          cleaned.substr(0, 3) +
          ") " +
          cleaned.substr(3, 3) +
          "-" +
          cleaned.substr(6, 4);
      }
    },
  },
};
</script>
