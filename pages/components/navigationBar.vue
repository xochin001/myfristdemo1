<template>
  <div class="comp-navbar">
    <!-- 占位栏 -->
    <cover-view class="placeholder-bar" :style="{height: navBarHeight + 'px'}"> </cover-view>
    <!-- 导航栏主体 -->
    <cover-view class="navbar" :style="{height: navBarHeight + 'px'}">
      <cover-image class="navbar" :style="{height: navBarHeight + 'px'}" :src="srcImg"></cover-image>
      <!-- 状态栏 -->
      <cover-view class="nav-statusbar" :style="{height: statusBarHeight + 'px'}"></cover-view>
      <!-- 标题栏 -->
      <cover-view class="nav-titlebar" :style="{height: titleBarHeight + 'px' }">
        <cover-view class="opt opt-home">
          <cover-image class="home-image" src="../../static/logo.png" ></cover-image>
        </cover-view>
        <!-- 标题 -->
        <cover-view class="bar-title" :style="[{color:titleColor}]">{{title}}</cover-view>
      </cover-view>
    </cover-view>
  </div>
</template>
<script>
export default {
  props: {

    // 标题颜色
    titleColor: {
      default: "#000000"
    },
    //标题文字
    title: {
      required: false,
      default: " "
    }

  },
  data() {
    return {
      statusBarHeight: "", // 状态栏高度
      titleBarHeight: "", // 标题栏高度
      navBarHeight: "", // 导航栏总高度
      platform: "",
      model: "",
      brand: "",
      system: "",
      srcImg: "https://www.jingsoftware.com/img/221.png"
    }
  },
  beforeMount() {
    const self = this;
    wx.getSystemInfo({
      success(system) {
        console.log(`system:`, system);
        self.statusBarHeight = system.statusBarHeight;
        self.platform = system.platform;
        self.model = system.model;
        self.brand = system.brand;
        self.system = system.system;
        let platformReg = /ios/i;
        if (platformReg.test(system.platform)) {
          self.titleBarHeight = 44;
        } else {
          self.titleBarHeight = 48;
        }
        self.navBarHeight = self.statusBarHeight + self.titleBarHeight;
      }
    });
  }
};

</script>
<style lang="scss">
.comp-navbar {
  width: 100vw;

  .navbar {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;

    .nav-titlebar {
      //border: 1px solid green;
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;

      .opt {
        width: 190rpx;
        height: 190rpx;
        display: flex;
        align-items: center;
        justify-content: space-around;
        position: absolute;
        left: 7px;
      }

      .home-image {
        width: 80rpx;
        height: 80rpx;
      }
    }

    .bar-title {
      width: 45%;
      font-size: 14px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      text-align: center;
    }
  }
}
    .placeholder-bar {
      background-color: transparent;
     width: 100%;
}

</style>
