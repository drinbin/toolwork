<template>
  <div id="search_help">
    <ToolContainer title="帮你百度">
      <a-input
        allow-clear
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
    <ToolContainer title="教程地址" v-if="results"> 开发中 </ToolContainer>
    <!-- <div v-if="results" title="获取成功">
      <a :href="results" target="_blank" rel="noopener noreferrer">
        {{ results }}
      </a>
    </div> -->
  </div>
</template>

<script>
export default {
  name: "SearchHelp",
  head() {
    return {
      title: "帮你百度 - ToolWork",
    };
  },
  data() {
    return {
      results: "",
      content: "",
    };
  },
  computed: {
    url() {
      // return `${window.location.origin}/o/search_help?k=${encodeURIComponent(
      //   this.content
      // )}`;
      return "开发中~";
    },
  },
  methods: {
    createCourse() {
      console.log(this.content, "this.content");
      if (this.content) {
        this.results = this.url;
      } else {
        this.$message.warning("请输入内容");
      }
    },
  },
};
</script>

<style lang="scss" scoped>
#search_help {
  .btn {
    margin: 0 auto;
    margin-top: 8px;
    text-align: center;
    width: 150px;
  }
}
</style>
