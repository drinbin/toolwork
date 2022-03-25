<template>
  <a-button
    type="primary"
    icon="copy"
    size="small"
    class="flex"
    @click="handleCopy"
  >
    <slot />
  </a-button>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    text: {
      type: String,
      require: true,
    },
    message: {
      type: String,
      default: '复制成功',
    },
  },
  methods: {
    handleCopy() {
      this.$copyText(this.text).then(
        () => {
          this.$message.success(this.message);
        },
        () => {
          this.$message.error('复制失败，请手动选择复制');
        }
      );
    },
  },
};
</script>
