<template>
  <div id="home-main">
    <div class="home-main">
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
    </div>
    <div id="footer">
      <a-divider />
      <Footer class="footer" />
    </div>
  </div>
</template>

<script>
import Footer from '~/components/home/Footer.vue';
export default {
  name: 'HomeMain',
  components: {
    Footer,
  },
  data() {
    return {
      toolList: [
        {
          title: '二维码生成器',
          href: '/qrcode',
          introduce: '将网页或者内容转成二维码',
        },
        {
          title: '帮你百度',
          href: '/search_help',
          introduce: '演示怎么百度',
        },
        {
          title: '番茄时钟',
          href: '/tomato_clock',
          introduce: '高效的时间管理方法',
        },
      ],
    };
  },
  methods: {},
};
</script>

<style lang="scss" scoped>
$minWidth: 1280px;
#home-main {
  .home-main {
    min-height: calc(100vh - 160px);
    .tool-card {
      margin-bottom: 10px;
      .introduce {
        color: #9c9c9c;
      }
    }
  }
  #footer {
    position: sticky;
    z-index: 99;
    width: 1280px;
  }
  .footer {
    // padding-bottom: 5px;
  }
}
</style>
