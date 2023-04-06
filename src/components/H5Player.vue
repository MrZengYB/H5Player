<template>
  <div class="h5-player-container">
    <div id="player" />
  </div>
</template>

<script>
export default {
  name: "H5Player",
  components: {},
  data() {
    return {
      player: null,
      errorMsg: "",
      loading: false,
    };
  },
  computed: {
    selectCenter() {
      return this.$store.state.selectCenter;
    },
  },
  mounted() {
    this.init();
    this.createPlayer();
    this.play();
  },
  methods: {
    init() {
      // 设置播放容器的宽高并监听窗口大小变化
      window.addEventListener("resize", () => {
        this.player.JS_Resize();
      });
    },
    createPlayer() {
      this.player = new window.JSPlugin({
        szId: "player",
        szBasePath: "/h5player",
        iMaxSplit: 1,
        iCurrentSplit: 1,
        iWidth: "100%",
        iHeight: "100%",
        openDebug: true,
        oStyle: {
          borderSelect: "#000",
        },
      });

      // 事件回调绑定
      this.player.JS_SetWindowControlCallback({
        pluginErrorHandler: function (iWndIndex, iErrorCode, oError) {
          //插件错误回调
          console.log("pluginError callback: ", iWndIndex, iErrorCode, oError);
        },
        windowFullCcreenChange: function (bFull) {
          //全屏切换回调
          console.log("fullScreen callback: ", bFull);
        },
        firstFrameDisplay: function (iWndIndex, iWidth, iHeight) {
          //首帧显示回调
          console.log(
            "firstFrame loaded callback: ",
            iWndIndex,
            iWidth,
            iHeight
          );
        },
      });
    },
    async play() {
      console.log("===========");
      // if (!this.props.playURL) {
      //   this.errorMsg.value = "无视频链接！";
      //   return false;
      // }
      this.errorMsg = "";
      this.loading = true;
      try {
        const _url =
          "wss://068y-argus.sf-express.com:6014/proxy/10.23.1.56:559/openUrl/F1mKiru";
        // 预览
        await this.player.JS_Play(
          _url,
          { playURL: _url, mode: 0 },
          1
          // this.props.startTime + "Z",
          // this.props.endTime + "Z"
        );
        // setIsPause(false);
        // setIsStop(false);
        // if (props.startTime && props.endTime) {
        //   computCurrentTime();
        // }
        return true;
      } catch (e) {
        // handleError(e, "视频播放失败！请重试。");
        console.log("===========e", e);
      }
      return false;
    },
  },
};
</script>

<style lang="scss" scoped>
.h5-player-container {
  position: relative;
  width: 100%;
  height: 50vw;
  background: #000;
  .version-tip {
    position: absolute;
    top: 20px;
    z-index: 1;
    margin: 20px;
    color: red;
    font-size: 18px;
  }
  .loading-spin {
    position: absolute;
    top: 0;
    width: 100%;
    height: 100%;
    .error-msg-box {
      width: 100%;
      color: red;
      font-size: 16;
      text-align: center;
      transform: translate(-50%, -70px);
    }
    & > div {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -70%);
      & > span {
        color: #ddd;
        font-size: 50px;
        cursor: pointer;
      }
    }
  }
  .control-bar-bg,
  .control-bar {
    position: absolute;
    bottom: 2px;
    left: 1px;
    width: calc(100% - 2px);
    height: 30px;
  }
  .control-bar-bg {
    z-index: 1;
    background-color: #fff;
    opacity: 0.3;
  }
  .control-bar {
    z-index: 2;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 6px;
    .full-screen,
    .play-btn {
      & > span {
        font-size: 24px;
        cursor: pointer;
      }
    }
    .progress-bar {
      display: flex;
      flex: 1;
      align-items: center;
      height: 25px;
      padding: 0 10px;
      & > span {
        margin: 0 5px;
      }
      .progress-slider {
        flex: 1;
      }
      .video-speed {
        position: relative;
        cursor: pointer;
        line-height: 30px;
        &:hover {
          .speed-list {
            display: block;
          }
        }
        .speed-list {
          display: none;
          position: absolute;
          top: -70px;
          left: -3px;
          width: 36px;
          text-align: center;
          background: #fff;
          border-radius: 5px;
          opacity: 0.7;
          line-height: 22px;
          padding: 2px 0px;
          &:hover {
            display: block;
          }
          .active {
            background-color: #ffcc00;
          }
          div {
            &:hover {
              background-color: #ddd;
            }
          }
        }
      }
    }
  }
}
</style>
