<template>
  <div id="tomato_clock">
    <ToolContainer title="番茄时钟">
      <a-list
        class="clock-list"
        item-layout="horizontal"
        :locale="{
          emptyText: '暂无计划',
        }"
        :data-source="clockList"
      >
        <div
          slot="loadMore"
          :style="{
            textAlign: 'center',
            marginTop: '12px',
            height: '32px',
            lineHeight: '32px',
          }"
        >
          <a-button @click="addPlan"> 增加计划 </a-button>
        </div>
        <a-list-item slot="renderItem" slot-scope="item, index">
          <span slot="actions" class="primary-color" @click="clickEdit(index)"
            >修改</span
          >
          <div type="link" slot="actions">
            <a-popconfirm
              placement="right"
              ok-text="删除"
              cancel-text="取消"
              okType="danger"
              @confirm="confirm"
            >
              <a-icon slot="icon" type="question-circle-o" style="color: red" />
              <template slot="title"> 确认删除该计划吗 </template>
              <span class="danger-color">删除</span>
            </a-popconfirm>
          </div>
          <a-list-item-meta :description="item.content">
            <a slot="title">第 {{ index + 1 }} 个计划</a>
          </a-list-item-meta>
          <div>
            专注
            <span class="secondary-color"> {{ item.workTime }} </span>
            分钟，休息
            <span class="secondary-color"> {{ item.restTime }} </span> 分钟
          </div>
        </a-list-item>
      </a-list>
      <div class="flex condition">
        <div class="condition-box">
          <span class="condition-label">全屏</span>
          <a-switch v-model="fullScreen" size="small"> </a-switch>
        </div>
        <div class="condition-box">
          <span class="condition-label">自动开始下一个计划</span>
          <a-switch v-model="autoPlan" size="small"> </a-switch>
        </div>
      </div>

      <div class="btn">
        <a-button type="primary" @click="handleStart">开始专注</a-button>
      </div>
    </ToolContainer>
    <ToolIntro text="番茄工作法">
      <li>
        开始专注之前先设置好计划，将想要完成的任务、专注时间、休息时间填写在上面的列表
      </li>
      <li>
        推荐以25分钟为一个番茄钟周期，周期开始时计时器会计时，全身心投入手中的工作中。每次完成一个目标，都会增加工作完成的满足感。
      </li>
      <li>
        当时间到达时，停下手中的工作，好好休息，激发下个番茄钟工作的动力。
      </li>
      <li>每4个番茄钟结束后进行较长时间休息，时间一般为15~25分钟。</li>
    </ToolIntro>

    <div
      v-if="showBox"
      ref="box"
      :key="bgColor"
      class="tomato-box"
      :class="successful ? 'green' : 'origin'"
    >
      <!-- <div v-if="clockList[currentIndex]" class="tomato-top">
        {{ clockList[currentIndex].content }}
      </div>
      <div v-else>-</div> -->
      <CircleAnimation :progress="progress">
        <div class="cancel" @click="cancel">结 束</div>
        <div class="time">{{ time }}</div>
      </CircleAnimation>
      <div
        v-if="!autoPlan && successful"
        class="p-btn"
        :style="{
          transform: !autoPlan && successful ? 'translateY(50px)' : '',
        }"
        @click="next"
      >
        下一个
      </div>
    </div>

    <a-modal
      v-model="editVisible"
      :title="`第 ${currentIndex + 1} 个计划`"
      @ok="handleOk"
    >
      <a-form-model :model="visibleForm">
        <a-form-model-item label="计划内容" prop="content">
          <a-input
            v-model="visibleForm.content"
            placeholder="填写以帮助高效完成计划"
          />
        </a-form-model-item>
        <a-form-model-item label="专注时间 (分钟)" prop="content">
          <a-input-number
            v-model="visibleForm.workTime"
            style="width: 100%"
            :min="0"
            :max="9999"
            placeholder="请填写专注时间"
          />
        </a-form-model-item>
        <a-form-model-item label="休息时间 (分钟)" prop="restTime">
          <a-input-number
            v-model="visibleForm.restTime"
            style="width: 100%"
            :min="0"
            :max="9999"
            placeholder="请填写休息时间"
          />
        </a-form-model-item>
      </a-form-model>
    </a-modal>
  </div>
</template>

<script>
class plan {
  constructor() {
    this.content = '专注一下';
    this.workTime = 25;
    this.restTime = 5;
  }
}
import CircleAnimation from '~/components/CircleAnimation';
export default {
  name: 'TomatoClock',
  head() {
    return {
      title: '番茄时钟 - ToolWork',
    };
  },
  components: { CircleAnimation },
  data() {
    return {
      time: '00:00',
      progress: 0,
      currentIndex: 0,
      interval: null,
      fullScreen: true,
      autoPlan: true,
      showBox: false,
      editVisible: false,
      visibleForm: {},
      clockList: [new plan()],
    };
  },
  computed: {
    bgColor() {
      const color = this.successful ? '#00b482' : '#f74b38';
      return color;
    },
  },
  methods: {
    addPlan() {
      if ((this.clockList.length + 1) % 4 === 0) {
        let clockObj = new plan();
        clockObj.restTime = 15;
        this.clockList.push(clockObj);
      } else {
        this.clockList.push(new plan());
      }
    },
    clickEdit(index) {
      this.visibleForm = { ...this.clockList[index] };
      this.currentIndex = index;
      this.editVisible = true;
    },
    handleOk() {
      this.clockList[this.currentIndex] = this.visibleForm;
      this.editVisible = false;
    },
    confirm() {
      this.clockList.splice(this.currentIndex, 1);
    },
    cancel() {
      clearInterval(this.interval);
      this.showBox = false;
    },
    handleStart() {
      if (this.clockList.length > 0) {
        this.showBox = true;
        this.successful = false;
        this.launchFullscreen();
        let time = this.clockList[this.currentIndex].workTime * 60 - 0.1;
        let past = 0;
        let rest = false;
        const startPlan = () => {
          if (time / 60 <= 0.1 && time % 60 <= 0.1) {
            if (this.autoPlan) {
              this.progress = 0;
              past = 0;
              if (!rest) {
                time = this.clockList[this.currentIndex].restTime * 60 - 0.1;
              } else {
                time = this.clockList[this.currentIndex].workTime * 60 - 0.1;
              }
              this.successful = !this.successful;
              rest = !rest;
            } else {
              clearInterval(this.interval);
              this.successful = true;
              this.time = '完 成';
              this.progress = 100;
              return false;
            }
          }
          const minutes = parseInt(time / 60);
          const seconds = parseInt(time % 60);
          this.progress =
            (past /
              ((rest
                ? this.clockList[this.currentIndex].restTime
                : this.clockList[this.currentIndex].workTime) *
                60 -
                1)) *
            100;
          if (minutes === 0 && seconds === 0) {
            this.progress = 100;
            // this.currentIndex += 1;
          }
          this.time = `${minutes > 9 ? minutes : '0' + minutes}:${
            seconds > 9 ? seconds : '0' + seconds
          }`;
          time -= 0.1;
          past += 0.1;
        };
        startPlan();
        this.interval = setInterval(startPlan, 100);
      } else {
        this.$message.warning('先去添加计划吧');
      }
    },
    launchFullscreen() {
      if (!this.fullScreen) return false;
      this.$nextTick(() => {
        const el = this.$refs.box;
        if (el.requestFullscreen) {
          el.requestFullscreen();
        } else if (el.mozRequestFullScreen) {
          el.mozRequestFullScreen();
        } else if (el.msRequestFullscreen) {
          el.msRequestFullscreen();
        } else if (el.webkitRequestFullscreen) {
          el.webkitRequestFullScreen();
        }
      });
    },
  },
};
</script>

<style lang="scss">
#tomato_clock {
  .clock-list {
    margin-bottom: 10px;
  }
  .condition {
    margin-bottom: 5px;
    &-box {
      display: flex;
      align-items: center;
    }
    &-label {
      margin-right: 5px;
    }
  }
  .circle-animation {
    transition: all 0.5s ease;
    .time {
      transition: all 0.5s ease;
    }
    .cancel {
      z-index: 1;
      cursor: pointer;
      position: absolute;
      width: 100%;
      height: 100%;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      background-color: rgba($color: #ffffff, $alpha: 0.2);
      transition: all 0.5s ease;
      font-size: 40px;
      &:hover {
        opacity: 1;
        & + .time {
          opacity: 0;
        }
      }
    }
  }
  .tomato-box {
    user-select: none;
    z-index: 100;
    position: fixed;
    left: 0;
    top: 0;
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    .info {
      color: #ffffff;
      margin-top: 20px;
      font-size: 18px;
      font-weight: bold;
    }
    .p-btn {
      position: absolute;
      top: calc(50vh + 160px);
      cursor: pointer;
      color: #ffffff;
      font-size: 18px;
      margin-top: 20px;
      border: 2px solid #ffffff;
      padding: 8px 0;
      border-radius: 20px;
      transition: all 0.5s ease;
      box-sizing: border-box;
      text-align: center;
      width: 110px;
      &:hover {
        background-color: rgba($color: #ffffff, $alpha: 0.2);
      }
    }
  }
  .origin {
    background-color: #f74b38;
  }
  .green {
    background-color: #00b482;
  }
}
</style>
