<template>
  <div>
    <h2 v-if="error.statusCode == 404">404 - 页面不存在</h2>
    <h2 v-else>500 - 服务器错误</h2>
    <nuxt-link to="/">回到首页</nuxt-link>
  </div>
</template>

<script>
export default {
  props: ["error"],
  head() {
    return {
      title: "似乎走丢了 - ToolWork",
    };
  },
};
</script>
