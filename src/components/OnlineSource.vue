<template lang="pug">
  div(class="onlinesrc_page")
    div(class="onlinesrc_background")
    div(class="onlinesrc_title")
    div(class="onlinesrc_logo")
    div(class="onlinesrc_flower_top")
    div(class="onlinesrc_flower_down")
    div(class="onlinesrc_back")
      router-link(tag="button" to="/" class="onlinesrc_exit_button")
    div(class="onlinesrc_layout")
</template>

<script>
export default {
  data: function () {
    return {
      currentIndex: -1,
      contents: [
        {
          Title: '我的筆記',
          URL: '../assets/14/parent/article_1.html',
          content: ``
        }
      ]
    }
  },
  mounted: function () {
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
    .onlinesrc_page {
      position: absolute;
      display: flex;
      align-items: center;
      justify-content: flex-start;
      justify-items: flex-start;
      height: 100vh;
      width: 100vw;
      background: white;
      margin: 0;
      padding: 0;
      overflow: hidden;
    }
    .onlinesrc_title {
      position: absolute;
      z-index: 20;
      left: 18vw;
      top: 8vh;
      width: 60vw;
      height: 30vw;
      background-image: url("../assets/14/news/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: 50% 50%;
    }
    .onlinesrc_back {
      position: absolute;
      z-index: 2;
      right: 2vw;
      top: 3vh;
      .onlinesrc_exit_button {
        width: 10vw;
        height: 10vw;
        background-color: transparent;
        background-image: url("../assets/exit.svg");
        background-repeat: no-repeat;
        background-size: 100% 100%;
        background-position: 50% 50%;
        margin: 3.5vw 4.2vw;
        outline: none;
        border: none;
        transition: filter .3s ease;
        cursor: pointer;
        filter: brightness(130%);
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
    .onlinesrc_page {
      position: absolute;
      display: flex;
      align-items: center;
      justify-content: flex-start;
      justify-items: flex-start;
      height: 100vh;
      width: 100vw;
      background:white;
      margin: 0;
      padding: 0;
      overflow: hidden;
    }
    .onlinesrc_background {
      position: absolute;
      z-index: 1;
      top: 0;
      right: -10vw;
      background: rgb(255, 246, 232);
      height: 100vh;
      width: 50vw;
      transform: skewX(5deg);
    }
    .onlinesrc_title {
      position: absolute;
      z-index: 20;
      left: 10vw;
      top: 6vh;
      width: 17vw;
      height: 8vw;
      background-image: url("../assets/14/news/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: 50% 50%;
    }
    .onlinesrc_logo {
      position: absolute;
      background-image: url('../assets/14/pageLogo.svg');
      background-repeat: no-repeat;
      background-position: center bottom 40%;
      background-size: 55% 55%;
      z-index: 2;
      right: 0vw;
      top: 0vw;
      width: 40vw;
      height: 100vh;
    }
    .onlinesrc_flower_top {
      position: absolute;
      background-image: url('../assets/14/flower.svg');
      background-repeat: no-repeat;
      background-position: center top 5%;
      background-size: cover;
      z-index: 2;
      right: 0vw;
      top: 0vw;
      width: 41vw;
      height: 40vh;
    }
    .onlinesrc_flower_down {
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
    .onlinesrc_layout {
      display: grid;
      // grid-template-rows: 1fr 4fr;
      // grid-template-columns: 1fr 4fr 1fr;
      // grid-template-areas: "title . exit" ". contents .";
      width: 100vw;
      height: 100vh;
    }
    .onlinesrc_title {
      position: absolute;
      z-index: 20;
      left: 10vw;
      top: 8vh;
      width: 17vw;
      height: 8vw;
      background-image: url("../assets/14/online/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: 50% 50%;
    }
    .onlinesrc_back {
      position: absolute;
      z-index: 2;
      right: 2vw;
      top: 3vh;
      .onlinesrc_exit_button {
        width: 6vw;
        height: 6vw;
        background-color: transparent;
        background-image: url("../assets/exit.svg");
        background-repeat: no-repeat;
        background-size: 100% 100%;
        background-position: 50% 50%;
        margin: 3.5vw 4.2vw;
        outline: none;
        border: none;
        transition: filter .3s ease;
        cursor: pointer;
        filter: brightness(130%);
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
