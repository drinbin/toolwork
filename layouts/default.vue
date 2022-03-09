<template>
  <div>
    <div id="topbar">
      <TopBar class="tobar" />
    </div>
    <div id="main">
      <nuxt class="main" />
    </div>
    <!-- <div id="footer">
      <Footer class="footer" />
    </div> -->
  </div>
</template>

<script>
import TopBar from "../components/home/TopBar";
import Footer from "../components/home/Footer";
import ToolContainer from "~/components/ToolContainer";
import ToolIntro from "~/components/ToolIntro";
export default {
  name: "Index",
  components: {
    TopBar,
    Footer,
    ToolContainer,
    ToolIntro,
  },
};
</script>

<style lang="scss" scoped>
$minWidth: 1280px;
#topbar {
  z-index: 99;
  position: fixed;
  top: 0;
  background: #fff;
  height: 50px;
  line-height: 50px;
  box-shadow: 0 0 8px rgb(0 0 0 / 10%);
  width: 100%;
  min-width: $minWidth;
}
.tobar {
  width: 1280px;
  margin: 0 auto;
  padding: 0 50px;
}
#main {
  min-height: 100%;
  min-width: $minWidth;
  position: relative;
  padding: 15px 0;
}
.main {
  height: 100%;
  width: 1280px;
  margin: 50px auto 0;
  position: relative;
  padding: 0 50px;
}
#footer {
  z-index: 99;
  position: fixed;
  bottom: 0;
  background: #fff;
  height: 40px;
  line-height: 40px;
  box-shadow: 0 0 8px rgb(0 0 0 / 10%);
  width: 100%;
  min-width: $minWidth;
}
.footer {
  width: 1280px;
  margin: 0 auto;
  padding: 0 50px;
}
</style>
