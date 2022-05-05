<template>
  <a-config-provider :locale="locale">
    <div>
      <div id="topbar">
        <TopBar class="tobar" />
      </div>
      <div id="main">
        <nuxt class="main" />
      </div>
    </div>
  </a-config-provider>
</template>

<script>
import zhCN from 'ant-design-vue/lib/locale-provider/zh_CN';
import TopBar from '~/components/home/TopBar';
import ToolContainer from '~/components/ToolContainer';
import ToolIntro from '~/components/ToolIntro';
export default {
  name: 'Index',
  components: {
    TopBar,
    ToolContainer,
    ToolIntro,
  },
  data() {
    return {
      locale: zhCN,
    };
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
  height: 100%;
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
</style>
