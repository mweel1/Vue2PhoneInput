<script>
import Vue from "vue";

export default Vue.extend({
  name: "ServeDev",
  data() {
    return {
      phone: "714-555-1212",
    };
  },
});
</script>

<template>
  <div id="app">
    {{ phone }}
    <vue2-phone-input v-model="phone" />

    <button @click="phone = '(949) 555-1213'">Set Phone</button>
  </div>
</template>
