<template>
  <div id="search_help">
    <div ref="mouse" class="mouse">
      <img
        v-if="cursorStatus"
        src="~/assets/images/search_help/mouse_cursor.svg"
      />
      <img v-else src="~/assets/images/search_help/mouse_pointer.svg" />
    </div>
    <div class="search-box">
      <img
        class="logo"
        src="~/assets/images/search_help/Baidu.png"
        width="270px"
      />
      <div class="search-input">
        <input
          ref="input"
          :value="inputVal"
          type="search"
          class="input-box"
          :class="step === 2 ? 'force' : ''"
        />
        <button id="su" ref="searchIcon">百度一下</button>
      </div>
    </div>
    <div class="info">
      {{ info[step - 1] }}
    </div>
    <div class="footer">
      <span>怎么百度 - BY</span>
      <nuxt-link class="primary-color" to="/" target="_blank"
        >ToolWork</nuxt-link
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchHelpDemo",
  head() {
    return {
      title: "怎么百度 - ToolWork",
    };
  },
  data() {
    return {
      key: "",
      step: 0,
      inputVal: "",
      cursorStatus: true,
      info: [
        "第一步：首先打开百度，点击搜索框",
        "第二步：输入想要查找的内容",
        "第三步：点击百度一下，立即前往",
        "不难吧，这不轻轻松松",
      ],
    };
  },
  computed: {
    url() {
      let key = this.$route.query.key;
      return `https://www.baidu.com/s?wd=${key}`;
    },
  },
  mounted() {
    if (this.$route.query.key) {
      this.key = this.$route.query.key;
      this.step1();
    } else {
      this.$message.error("演示失败，请重新生成");
    }
  },
  methods: {
    step1() {
      this.step++;
      let inputXY = this.$refs.input.getBoundingClientRect();
      this.$refs.mouse.style.transform = `translateX(${
        inputXY.left + 20
      }px) translateY(${inputXY.top + 5}px)`;
      setTimeout(this.step2, 2000);
    },
    step2() {
      this.step++;
      this.$refs.mouse.style.opacity = 0.3;
      this.$refs.input.focus();
      setTimeout(() => {
        let string = this.key.split("");
        let keyTime = string.length * 200;
        string.forEach((e, i) => {
          setTimeout(() => {
            this.inputVal = this.key.slice(0, i + 1);
          }, 200 * i);
        });
        setTimeout(this.step3, keyTime + 1000);
      }, 500);
    },
    step3() {
      let searchIconXY = this.$refs.searchIcon.getBoundingClientRect();
      this.$refs.mouse.style.opacity = 1;
      setTimeout(() => {
        this.$refs.mouse.style.transform = `translateX(${
          searchIconXY.left + 20
        }px) translateY(${searchIconXY.top}px)`;
        setTimeout(() => {
          this.cursorStatus = false;
        }, 750);
        setTimeout(this.step4, 2000);
      }, 800);
    },
    step4() {
      this.step++;
      setTimeout(() => {
        window.location.href = this.url;
      }, 1000);
    },
  },
};
</script>

<style lang="scss">
* {
  cursor: none;
}
body {
  background-color: #fff;
}
#topbar,
#footer {
  display: none;
}
#search_help {
  user-select: none;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  flex-direction: column;
  box-sizing: border-box;
  .logo {
    display: block;
    margin: 20px auto;
  }
  .search-box {
    width: 700px;
    max-width: 100%;
    input {
      display: inline-block;
      padding: 12px 20px;
      outline: none;
      border: none;
      background-color: transparent;
      box-sizing: border-box;
    }
    .search-input {
      display: flex;
      align-items: center;
      transition: all 0.3s ease;
      .input-box {
        border: 2px solid #c4c7ce;
        border-radius: 10px 0 0 10px;
        border-right: 0;
        &.force {
          border: 2px solid #4e6ef2;
        }
      }
      input {
        width: calc(100% - 60px);
      }
      i {
        display: inline-block;
        font-size: 20px;
      }
      #su {
        cursor: pointer;
        width: 108px;
        height: 44px;
        line-height: 45px;
        line-height: 44px\9;
        padding: 0;
        background: 0 0;
        background-color: #4e6ef2;
        border-radius: 0 10px 10px 0;
        font-size: 17px;
        color: #fff;
        box-shadow: none;
        font-weight: 400;
        border: none;
        outline: 0;
      }
    }
  }
  .mouse {
    position: fixed;
    left: 0;
    top: 0;
    font-size: 40px;
    transition: linear all 1s;
  }
  .info {
    margin-top: 50px;
    font-size: 25px;
  }
  .footer {
    position: fixed;
    text-align: center;
    background-color: #fff;
    left: 0;
    bottom: 0;
    padding-bottom: 20px;
    width: 100%;
    font-size: 20px;
  }
}
</style>
