<template>
  <div id="tool-page">
    <div class="tool-container">
      <div class="title">
        {{ title }}
      </div>
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  name: "ToolContainer",
  props: {
    title: {
      type: String,
      default: "",
    },
  },
};
</script>

<style lang="scss" scoped>
#tool-page {
  .tool-container {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 12px 20px;
    margin-bottom: 30px;
  }
  .title {
    font-size: 18px;
    display: table;
    margin-bottom: 10px;
  }
  .title::after {
    content: "";
    margin-top: 3px;
    width: 80%;
    display: block;
    border-radius: 5px;
    border: 2px solid #108775;
  }
}
</style>
