<template>
  <div id="HomeHeader">
    <div class="head">
      <nuxt-link to="/" class="logo">
        <img src="~/assets/images/logo.png" alt="ToolWork" class="logo-icon" />
      </nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TopBar',
  data() {
    return {};
  },
};
</script>

<style lang="scss" scoped>
#HomeHeader {
  .head {
    display: flex;
    height: 100%;
  }
  .logo {
    display: flex;
    align-items: center;
    &-icon {
      width: 150px;
      user-select: none;
    }
  }
}
</style>
