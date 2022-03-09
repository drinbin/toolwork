<template>
  <div id="HomeHeader">
    <div style="display: flex">
      <nuxt-link to="/"
                 class="logo">
        <img src="~/assets/images/logo.png"
             alt="ToolWork"
             class="logo-icon" />
        <div class="logo-title">
          <span>Tool</span><span class="primary-color">Work</span>
        </div>
      </nuxt-link>
    </div>
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
  a:hover {
    color: #2b3e4e;
  }
  .logo {
    display: flex;
    align-items: center;
    &-icon {
      width: 32px;
      user-select: none;
    }
    &-title {
      padding-left: 6px;
      font-size: 26px;
    }
  }
}
</style>
