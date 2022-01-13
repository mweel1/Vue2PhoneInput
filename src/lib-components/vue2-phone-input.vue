<template>
  <div>
    <input
      type="input"
      v-bind="$attrs"
      v-model="phone"
      @keyDown="keyDown"
      maxlength="14"
    />
  </div>
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
  },
  created() {
    this.phone = this.value;

    if (this.phone) this.doFormat();
  },
  methods: {
    keyDown(e) {
      var curchr = this.phone.length;

      if (e.which != 8 && e.which != 0 && (e.which < 48 || e.which > 57)) {
        e.preventDefault();
        return;
      }

      if (curchr == 0 && e.key == "1") {
        e.preventDefault();
        return;
      }

      if (e.which == 8) {
        return;
      }

      this.doFormat();
    },
    doFormat() {
      //Filter only numbers from the input
      let cleaned = ("" + this.phone).replace(/\D/g, "");

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
