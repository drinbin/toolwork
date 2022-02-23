<template>
  <div id="HomeHeader">
    <nuxt-link to="/">
      <div class="logo">
        <img src="~/assets/logo.png" alt="ToolWork" class="logo-icon" />
        <div class="logo-title">
          <span>Tool</span><span class="primary-color">Work</span>
        </div>
      </div>
    </nuxt-link>
  </div>
</template>

<script>
export default {
  name: "TopBar",
  data() {
    return {};
  },
};
</script>

<style lang="scss" scoped>
#HomeHeader {
  .logo {
    display: flex;
    align-items: center;
    &-icon {
      width: 32px;
    }
    &-title {
      margin-left: 6px;
      font-size: 26px;
    }
  }
}
</style>
