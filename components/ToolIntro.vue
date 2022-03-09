<template>
  <div id="tool-intro">
    <a-collapse default-active-key="1">
      <a-collapse-panel key="1"
                        :header="text">
        <ul>
          <slot />
        </ul>
      </a-collapse-panel>
    </a-collapse>
  </div>
</template>

<script>
export default {
  name: "ToolIntro",
  props: {
    text: {
      type: String,
      default: "说明",
    },
  },
};
</script>

<style lang="scss" scoped>
#tool-intro {
  margin-bottom: 30px;
}
</style>
