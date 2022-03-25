<template>
  <div id="search_help">
    <ToolContainer title="帮你百度">
      <div class="number">{{ content.length }} / {{ maxLength }}</div>
      <a-input
        allow-clear
        :maxLength="maxLength"
        v-model.trim="content"
        placeholder="怎么发家致富"
        @pressEnter="createCourse"
      >
      </a-input>
      <div class="btn">
        <a-button type="primary" block @click="createCourse">生成教程</a-button>
      </div>
    </ToolContainer>
    <ToolIntro>
      <li>问题太easy，不想回答？用这个工具教他百度吧~</li>
    </ToolIntro>
    <ToolContainer title="教程地址" v-if="results">
      <a :href="results" target="_blank" rel="noopener noreferrer">
        {{ results }}
      </a>
      <CopyIcon class="ml-5" :text="results">复制链接</CopyIcon>
    </ToolContainer>
  </div>
</template>

<script>
import CopyIcon from '~/components/CopyIcon';
export default {
  name: 'SearchHelp',
  components: {
    CopyIcon,
  },
  head() {
    return {
      title: '帮你百度 - ToolWork',
    };
  },
  data() {
    return {
      results: '',
      content: '',
      maxLength: 50,
    };
  },
  computed: {
    url() {
      return `${
        window.location.origin
      }/demonstration/search_help?key=${encodeURIComponent(this.content)}`;
    },
  },
  methods: {
    createCourse() {
      if (this.content) {
        this.results = this.url;
      } else {
        this.$message.warning('请输入内容');
      }
    },
  },
};
</script>

<style lang="scss" scoped>
#search_help {
  .number {
    margin-bottom: 5px;
    text-align: right;
  }
  .btn {
    margin: 0 auto;
    margin-top: 8px;
    text-align: center;
    width: 150px;
  }
  .icon {
    margin-left: 5px;
  }
}
</style>
