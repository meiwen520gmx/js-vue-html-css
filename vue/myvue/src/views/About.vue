<template>
  <div class="about">
    <!-- 进度条 -->
    <div class="progress-bar">
      <div class="top-num" :style="{ paddingLeft: doneWidth }">
        <div class="text-bar">{{ value }}%</div>
        <div class="text-icon">
          <i class="el-icon-caret-bottom"></i>
        </div>
      </div>
      <div class="p-line"></div>
      <div class="p-done" :style="{ width: doneWidth }"></div>
      <div class="bottom-num">
        <div style="float:left">0</div>
        <div style="float:right">{{ max }}</div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      value: 0,
      max: 100,
      timer: null,
    };
  },
  computed: {
    doneWidth() {
      return (this.value / this.max) * 100 + "%";
    },
  },
  watch: {
    value(newValue, oldValue) {
      if (newValue >= this.max) {
        clearInterval(this.timer);
      }
    },
  },
  mounted() {
    this.timer = setInterval(() => {
      this.value += 5;
    }, 1000);
  },
};
</script>
<style>
.progress-bar {
  margin: 0 auto;
  width: 50%;
  position: relative;
}
.p-line {
  position: absolute;
  z-index: 1;
  width: 100%;
  height: 10px;
  border-radius: 4px;
  background-color: #eee;
}
.p-done {
  position: absolute;
  /* width: 80%; */
  z-index: 2;
  height: 10px;
  border-radius: 4px;
  background-image: linear-gradient(
    45deg,
    #40a9ff 25%,
    #1890ff 0,
    #1890ff 50%,
    #40a9ff 0,
    #40a9ff 75%,
    #1890ff 0
  );
  background-size: 30px 30px;
}
.top-num {
  margin-left: -30px;
  margin-right: -80px;
  text-align: left;
  padding-bottom: 10px;
}
.text-bar {
  font-size: 13px;
}
.text-icon {
  color: #008cdb;
  padding-left: 20px;
  margin-top: -5px;
}
/* .text-icon i {
  font-size: 18px;
} */
.bottom-num {
  width: 100%;
  padding-top: 15px;
}
</style>
