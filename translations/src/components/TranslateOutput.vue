<template>
  <!-- textarea for the translation -->
  <textarea
    disabled
    rows="10"
    style="width: 30%"
    type="text"
    v-model="translatedText"
  />
</template>

<script>
export default {
  name: "translateOutput",
  props: ["translatedText"],
};
</script>

<style></style>
