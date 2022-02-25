<template>
  <div id="qrcode">
    <ToolContainer title="二维码生成器">
      <div class="choose">
        <a-radio-group v-model="type">
          <a-radio-button value="text"> 文本 </a-radio-button>
          <a-radio-button value="net"> 网站 </a-radio-button>
        </a-radio-group>
        <div>
          {{ type === "text" ? textValue.length : netValue.length }} /
          {{ maxlength }}
        </div>
      </div>
      <a-textarea
        :maxLength="maxlength"
        v-show="type === 'text'"
        v-model.trim="textValue"
        placeholder="人生也一样，有白天和黑夜，只是不会像真正的太阳那样，有定时的日出和日落。"
        :auto-size="{ minRows: 3 }"
      />
      <a-input
        v-show="type === 'net'"
        :maxLength="maxlength"
        allow-clear
        v-model.trim="netValue"
        placeholder="www.toolwork.cn"
        @pressEnter="createCode"
      >
        <a-select
          slot="addonBefore"
          default-value="http://"
          @change="changeNet"
        >
          <a-select-option value="http://">http://</a-select-option>
          <a-select-option value="https://">https://</a-select-option>
        </a-select>
      </a-input>
      <div class="btn">
        <a-button type="primary" block @click="createCode">生成二维码</a-button>
      </div>
    </ToolContainer>
    <ToolIntro>
      <li>若二维码扫描不出来，可调整二维码图片大小</li>
    </ToolIntro>
    <ToolContainer title="二维码" v-show="picUrl !== ''">
      <div class="qrcode-img">
        <img alt="二维码" :src="picUrl" :width="`${170 * size}px`" />
      </div>
      <div class="size-container">
        <span>二维码图片大小：</span>
        <a-radio-group v-model="size">
          <a-radio :value="0.6"> 小 </a-radio>
          <a-radio :value="1"> 正常 </a-radio>
          <a-radio :value="1.4"> 大 </a-radio>
          <a-radio :value="1.8"> 特大 </a-radio>
        </a-radio-group>
      </div>
      <a-alert
        v-if="resultText"
        message="二维码解析"
        :description="resultText"
        type="info"
      />
    </ToolContainer>
  </div>
</template>

<script>
import QR from "qr-image";
export default {
  name: "qrcode",
  head() {
    return {
      title: "二维码生成器 - ToolWork",
    };
  },
  data() {
    return {
      textValue: "",
      netValue: "",
      picUrl: "",
      resultText: "",
      type: "text",
      size: 1,
      maxlength: 300,
      netType: "http://",
    };
  },
  computed: {
    qrcodeText() {
      let text = this.type === "text" ? this.textValue : this.netValue;
      return text;
    },
  },
  methods: {
    createCode() {
      if (this.qrcodeText) {
        if (this.type === "net") {
          const re =
            /^(((ht|f)tps?):\/\/)?([^!@#$%^&*?.\s-]([^!@#$%^&*?.\s]{0,63}[^!@#$%^&*?.\s])?\.)+[a-z]{2,6}\/?/;
          if (!re.test(this.qrcodeText)) {
            this.$message.warning("请填写正确的网站地址");
            return;
          }
        }
        let getText = this.qrcodeText;
        if (this.type === "net") getText = this.netType + getText;
        try {
          const QRData = QR.imageSync(getText, {
            type: "jpg",
            size: 6,
          });
          this.picUrl = `data:image/png;base64,` + QRData.toString("base64");
          this.resultText = getText;
        } catch (error) {
          this.$message.error("生成失败：" + error);
        }
      } else {
        this.$message.warning("请先填写内容");
      }
    },
    changeNet(value) {
      this.netType = value;
    },
  },
};
</script>

<style lang="scss">
#qrcode {
  .choose {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;
  }
  .btn {
    margin: 0 auto;
    margin-top: 8px;
    text-align: center;
    width: 150px;
  }
  .size-container {
    margin: 3px 0;
    text-align: center;
  }
  .qrcode-img {
    text-align: center;
    margin-bottom: 15px;
  }
}
</style>
