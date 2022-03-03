<template>
  <div id="home-main">
    <a-row :gutter="16">
      <a-col
        v-for="(item, index) in toolList"
        :key="index"
        :span="6"
        class="tool-card"
      >
        <nuxt-link :to="item.href" target="_blank">
          <a-card hoverable :title="item.title" :bordered="false">
            <span slot="extra" class="secondary-color">使用</span>
            <p class="introduce">{{ item.introduce }}</p>
          </a-card>
        </nuxt-link>
      </a-col>
    </a-row>
    开发中~
  </div>
</template>

<script>
export default {
  name: "HomeMain",
  data() {
    return {
      toolList: [
        {
          title: "二维码生成器",
          href: "/qrcode",
          introduce: "将网页或者内容转成二维码",
        },
        {
          title: "帮你百度",
          href: "/search_help",
          introduce: "演示怎么百度",
        },
        {
          title: "番茄时钟",
          href: "/tomato_clock",
          introduce: "高效的时间管理方法",
        },
      ],
    };
  },
  methods: {},
};
</script>

<style lang="scss" scoped>
#home-main {
  .tool-card {
    margin-bottom: 10px;
    .introduce {
      color: #9c9c9c;
    }
  }
}
</style>
