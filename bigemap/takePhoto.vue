// 调取微信摄像头组件
<template>
    <camera
        v-if="showCamera"
        class="camera"
        device-position="front"
        :flash="flash"
        @error="erro"
        @stop="stop"
    >
        <cover-view>
            <cover-image class="face-bg-img" :style="{'height': height}" src="/static/img/face-bg.png" />
            <cover-view class="take-photo" @click="takePhoto1"></cover-view>
        </cover-view>
</template>
<script>
export default {
  name: "camera",
  props: {
    // 传参值
    position: {
      type: String,
      default: "front",
    },
    flash: {
      type: String,
      default: "off",
    },
    quality: {
      type: String,
      default: "low",
    },
  },
  data() {
    return {
      showCamera: false,
      height: "auto,",
    };
  },
  watch: {
    showCamera() {
      if (this.showCamera) {
        this.$nextTick(() => {
          const query = wx.createSelectQuery();
          query.select(".face-bg-img").boundingClientRect();
          query.exec((res) => {});
        });
      }
    },
  },
  mounted() {},
  methods: {
    error() {
      console.log("用户不允许使用摄像头");
    },
    stop() {
      console.log("摄像头在非正常终止时");
    },
    // 拍照
    takePhoto1() {
      let that = this;
      const ctx = wx.createCameraContext();
      ctx.takePhoto({
        quality: this.quality,
        success(res) {
          console.log("success", res);
          this.showCamera = false;
        },
        fail() {
          console.log("failed");
        },
        complete() {
          console.log("complter takePhoto！");
        },
      });
    },
  },
  // 载入
  onLoad() {
    this.showCamera = false;
  },
};
</script>
<style lang="less">
.camera {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 99;
  .face-bg {
    width: 100%;
    height: 100%;
    .take-photo {
      position: absolute;
      left: 50%;
      bottom: 50px;
      width: 80px;
      height: 80px;
      border-radius: 50%;
      background-color: rgba(255, 255, 255, 0.5);
      transform: translateX(-50%);
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .face-bg-img {
      display: block;
      width: 100%;
      height: 100%;
    }
  }
}
</style>