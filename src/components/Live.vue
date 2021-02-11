<template lang="pug">
  div(class="live_page")
    div(class="live_background")
    div(class="live_title")
    div(class="live_logo")
    div(class="live_flower_top")
    div(class="live_flower_down")
    div(class="live_back")
      router-link(tag="button" to="/" class="live_exit_button")
</template>

<script>
//  import axios from 'axios'
export default {
  data: function () {
    return {
      news: [
        {
          Title: '官網正式上線',
          Content: '官網正式上線',
          Timestamp: '2021/2/15'
        }
      ],
      currentIndex: -1,
      loader: null
    }
  },
  mounted: async function () {
  },
  methods: {
  }
}
</script>

<style lang="scss" scoped>
  /*
    mobile layout css
  */
  @media only screen and (max-width: 551px) {
    .live_page {
      display: flex;
      align-items: center;
      justify-content: center;
      position: absolute;
      height: 100vh;
      width: 100vw;
      margin: 0;
      padding: 0;
      background: rgb(255, 246, 232);
    }
    .live_background {
      display: none;
    }
    .live_title {
      position: absolute;
      z-index: 20;
      top: 3vh;

      width: 45vw;
      height: 20vw;
      background-image: url("../assets/14/live/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: center top 10%;
    }
    .live_back {
      position: absolute;
      z-index: 2;
      right: 2vw;
      top: 3vh;
      .live_exit_button {
        width: 10vw;
        height: 10vw;
        background-color: transparent;
        background-image: url("../assets/14/home.svg");
        background-repeat: no-repeat;
        background-size: 100% 100%;
        background-position: 50% 50%;

        margin: 3.5vw 4.2vw;
        outline: none;
        border: none;
        transition: filter .3s ease;
        cursor: pointer;

        &:hover {
          filter: brightness(150%);
        }
        &:active {
          filter: brightness(80%);
        }
      }
    }
  }
  /*
    computer layout css
  */
  @media only screen and (min-width: 552px) {
    .live_page {
      position: absolute;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      width: 100vw;
      margin: 0;
      padding: 0;
      background: white;
      overflow: hidden;
    }
    .live_background {
      position: absolute;
      z-index: 1;
      top: 0;
      right: -10vw;
      background: rgb(255, 246, 232);
      height: 100vh;
      width: 50vw;
      transform: skewX(5deg);
    }
    .live_title {
      position: absolute;
      z-index: 20;
      left: 4vw;
      top: 6vh;

      width: 17vw;
      height: 8vw;
      background-image: url("../assets/14/live/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: 50% 50%;
    }
    .live_logo {
      position: absolute;
      background-image: url('../assets/14/pageLogo.svg');
      background-repeat: no-repeat;
      background-position: center bottom 40%;
      background-size: 50% 50%;
      z-index: 2;
      right: 0vw;
      top: 0vw;
      width: 40vw;
      height: 100vh;
    }
    .live_flower_top {
      position: absolute;
      background-image: url('../assets/14/flower.svg');
      background-repeat: no-repeat;
      background-position: center top;
      background-size: cover;
      z-index: 2;
      right: 0vw;
      top: -5vh;
      width: 41vw;
      height: 40vh;
    }
    .live_flower_down {
      position: absolute;
      background-image: url('../assets/14/flower.svg');
      background-repeat: no-repeat;
      background-position: center bottom;
      background-size: cover;
      z-index: 2;
      right: 0vw;
      bottom: -5vh;
      width: 41vw;
      height: 40vh;
    }

    .live_back {
      position: absolute;
      z-index: 2;
      right: 2vw;
      top: 3vh;
      z-index: 10;
      .live_exit_button {
        width: 6vw;
        height: 6vw;
        background-color: transparent;
        background-image: url("../assets/14/home.svg");
        background-repeat: no-repeat;
        background-size: 100% 100%;
        background-position: 50% 50%;

        margin: 3.5vw 4.2vw;
        outline: none;
        border: none;
        transition: filter .3s ease;
        cursor: pointer;

        &:hover {
          filter: brightness(150%);
        }
        &:active {
          filter: brightness(80%);
        }
      }
    }

  }
</style>
