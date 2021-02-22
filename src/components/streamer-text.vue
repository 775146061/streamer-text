<template>
  <div class="streamer-text" ref="wrapper">
    <span class="text">{{ text }}</span>
    <span class="difference-mode" ref="differenceText">{{ text }}</span>
    <span class="overlay-mode"></span>
    <span class="color-dodge-mode"></span>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";

export default defineComponent({
  name: "StreamerText",
  props: {
    text: {
      type: String,
      required: true
    },
    color: {
      type: String,
      default: "#fff"
    },
    backgroundColor: {
      type: String,
      default: "#0000FF"
    }
  },
  setup(props) {
    const wrapper = ref<HTMLDivElement>(null);
    const differenceText = ref<HTMLSpanElement>(null);

    // 简写的16进制颜色字符匹配正则
    const colorRE = /^#([\d\w]{3}|[\d\w]{6})$/i;
    onMounted(() => {
      const matchColor = colorRE.exec(props.color);
      let color = props.color;
      if (matchColor && matchColor[1].length === 3) {
        color = color + matchColor[1];
      }
      wrapper.value.style.color = props.color;
      wrapper.value.style.backgroundColor = props.backgroundColor;

      const matchBgColor = colorRE.exec(props.backgroundColor);
      let bgcolorHexStr
      const colorHexStr = color.substring(1);
      if (matchBgColor && matchBgColor[1].length === 3) {
        bgcolorHexStr = matchBgColor[1] + matchBgColor[1];
      } else if (matchBgColor) {
        bgcolorHexStr = matchBgColor[1];
      } else {
        throw new Error("backgroundColor格式不对!");
      }

      const diffColorNum =
        Number.parseInt(bgcolorHexStr, 16) - Number.parseInt(colorHexStr, 16);
      const diffColorHexStr = Math.abs(diffColorNum).toString(16);
      const replenish = '0'.repeat(6 - diffColorHexStr.length)
      differenceText.value.style.color = '#' + replenish + diffColorHexStr
    });

    return {
      wrapper,
      differenceText
    };
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
@font-face {
  font-family: 'Merienda';
  font-style: normal;
  font-weight: 400;
  src: local('Merienda'), local('Merienda-Regular_0_wt'), url('https://fonts.gstatic.font.im/s/merienda/v8/gNMHW3x8Qoy5_mf8uWMLMIqK_Q.woff2') format('woff2'); // fonts.gstatic.font.im/s/merienda/v8/gNMHW3x8Qoy5_mf8uWMLMIqK_Q.woff2) format('woff2');
  unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
}

/* latin */
@font-face {
  font-family: 'Merienda';
  font-style: normal;
  font-weight: 400;
  src: local('Merienda'), local('Merienda-Regular_0_wt'), url('https://fonts.gstatic.font.im/s/merienda/v8/gNMHW3x8Qoy5_mf8uWMFMIo.woff2') format('woff2'); // fonts.gstatic.font.im/s/merienda/v8/gNMHW3x8Qoy5_mf8uWMFMIo.woff2) format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

.streamer-text {
  position: relative;
  overflow: hidden;
  display: inline-block;
  background: #fff;
  font-family: 'Merienda', cursive;
  font-size: 168px;

  .text {
    display: inline-block;
    user-select: none;
  }

  .difference-mode {
    position: absolute;
    top: 0;
    left: 0;
    filter: blur(3px);
    mix-blend-mode: difference;
  }

  .overlay-mode {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, rgb(255, 0, 212), rgb(0, 119, 255), rgb(255, 187, 0), rgb(1, 255, 77));
    mix-blend-mode: overlay;
  }

  .color-dodge-mode {
    position: absolute;
    top: -100%;
    left: -100%;
    right: 0;
    bottom: 0;
    background-image: radial-gradient(circle, white, black 100%);
    background-size: 25% 25%;
    mix-blend-mode: color-dodge;
    animation: shine 3s linear infinite;
  }

  @keyframes shine {
    100% {
      transform: translate(50%, 50%);
    }
  }
}
</style>
