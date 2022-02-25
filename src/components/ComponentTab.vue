<template>
  <div class="comp-tab">
    <slot></slot>
  </div>
</template>
<script>
import { BIcon, BIconArrowUp, BIconArrowDown } from "bootstrap-vue";
export default {
  components: {
    BIcon,
    BIconArrowUp,
    BIconArrowDown
  },
  name: "comp-tab",
  data() {
    return {};
  }
};
</script>
<style></style>
