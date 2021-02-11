<template lang="pug">
  div(class="onlinesrc_page")
    //- div(class="onlinesrc_background")
    div(class="onlinesrc_title")
    //- div(class="onlinesrc_logo")
    div(class="onlinesrc_flower_top")
    div(class="onlinesrc_flower_down")
    div(class="onlinesrc_layout")
      section(class="onlinesrc_layout_strategy")
        label(class="onlinesrc_item_name") 備審攻略
        div(class="onlinesrc_item_background")
          div(class="onlinesrc_item_content")
            a(tag="label" class="onlinesrc_item_content_list" v-for="(iter, index) of contenta" v-bind:key="iter.text" v-bind:href="`${iter.link}`" target="_blank" v-bind:data-name="iter.name" v-bind:style="{'background-image': 'url(' + `${iter.img}` + ')'}" v-on:click="openTab(index)")
            label(class="onlinesrc_item_content_text" v-for="(iter, index) of contenta")
      section(class="onlinesrc_layout_interview")
        label(class="onlinesrc_item_name") 模擬面試
        div(class="onlinesrc_item_background")
          div(class="onlinesrc_item_content")
            a(tag="label" class="onlinesrc_item_content_list" v-for="(iter, index) of contenta" v-bind:key="iter.text" v-bind:href="`${iter.link}`" target="_blank" v-bind:data-name="iter.name" v-bind:style="{'background-image': 'url(' + `${iter.img}` + ')'}" v-on:click="openTab(index)")
            label(class="onlinesrc_item_content_text" v-for="(iter, index) of contenta")
      section(class="onlinesrc_layout_share")
        label(class="onlinesrc_item_name") 成大生活分享
        div(class="onlinesrc_item_background")
          div(class="onlinesrc_item_content")
            a(tag="label" class="onlinesrc_item_content_list" v-for="(iter, index) of contenta" v-bind:key="iter.text" v-bind:href="`${iter.link}`" target="_blank" v-bind:data-name="iter.name" v-bind:style="{'background-image': 'url(' + `${iter.img}` + ')'}" v-on:click="openTab(index)")
            label(class="onlinesrc_item_content_text" v-for="(iter, index) of contenta")
    div(class="onlinesrc_back")
      router-link(tag="button" to="/" class="onlinesrc_exit_button")
</template>

<script>
//  import axios from 'axios'
export default {
  data: function () {
    return {
      contenta: [
        {
          text: 'Chill cow',
          link: 'https://www.youtube.com/watch?v=5qap5aO4i9A',
          img: 'https://i.imgur.com/Hu5Z3Yq.png'
        },
        {
          text: 'old high',
          link: 'https://www.youtube.com/watch?v=3mBLjHxwwmo',
          img: 'https://i.imgur.com/jklIThX.png'
        },
        {
          text: 'old high',
          link: 'https://www.youtube.com/watch?v=3mBLjHxwwmo',
          img: 'https://i.imgur.com/jklIThX.png'
        },
        {
          text: 'old high',
          link: 'https://www.youtube.com/watch?v=3mBLjHxwwmo',
          img: 'https://i.imgur.com/jklIThX.png'
        }
      ],
      currentIndex: -1
    }
  },
  mounted: async function () {
    // const list = this.$el.querySelector()
    // list.classList.add('onlinesrc_item_content_text')
  },
  methods: {
    openTab: function (index) {
      window.open(this.sponsor[index].link)
    }
  }
}
</script>

<style lang="scss" scoped>
  /*
    mobile layout css
  */
  @media only screen and (max-width: 551px) {
    .onlinesrc_page {
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
    .onlinesrc_background {
      display: none;
    }
    .onlinesrc_title {
      position: absolute;
      z-index: 20;
      top: 3vh;

      width: 45vw;
      height: 20vw;
      background-image: url("../assets/14/online/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: center top 10%;
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
    .onlinesrc_page {
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
      left: 4vw;
      top: 6vh;

      width: 17vw;
      height: 8vw;
      background-image: url("../assets/14/online/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: 50% 50%;
    }
    .onlinesrc_logo {
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
    .onlinesrc_flower_top {
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
      position: absolute;
      z-index: 3;
      top: 21vh;
      width: 95vw;
      height: 75vh;
      overflow-y: scroll;
    }

    .onlinesrc_layout_strategy {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 95vw;
      height: 70vh;
      margin: 0 0 2.5vw 0;
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 30vw;
        height: auto;
        font-size: 6vh;
        color: rgb(103, 192, 225);
        line-height: 13vh;
      }
      .onlinesrc_item_background {
        justify-self: center;
        grid-area: content;
        display: flex;
        justify-content: center;
        align-items: center;
        background: rgb(241,207,217);
        width: 100%;
        .onlinesrc_item_content {
          width: 75vw;
          height: 50vh;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: flex-start;
          align-self: flex-end;
          grid-template-columns: repeat(4, 25vw);
          grid-template-rows: repeat(2, 16vw);
          overflow-x: scroll;
          .onlinesrc_item_content_list {
            background: black;
            border-radius: 2vw 2vw 0 0;
            width: 21vw;
            height: 16vw;
            z-index: 20;
            &:hover {
              filter: brightness(150%);
              color: white;
              text-align: center;
            }
            &:active {
              filter: brightness(50%);
              content: attr(data-name);
            }
          }
          .onlinesrc_item_content_text {
            background: rgb(62,181,231);
            border-radius: 0 0 2vw 2vw;
            content: attr(data-name);
            width: 21vw;
            height: 7vw;
            z-index: 21;
          }
        }
      }
    }

    .onlinesrc_layout_interview {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 95vw;
      height: 70vh;
      margin: 0.5vh 0 0.5vh 0;
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 30vw;
        height: 15vh;
        font-size: 6vh;
        color: rgb(103, 192, 225);
        line-height: 13vh;
      }
      .onlinesrc_item_background {
        justify-self: center;
        grid-area: content;
        display: flex;
        justify-content: center;
        align-items: center;
        background: rgb(254,241,217);
        width: 100%;
        .onlinesrc_item_content {
          width: 81vw;
          height: 50vh;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: flex-start;
          align-self: flex-end;
          grid-template-columns: repeat(4, 27vw);
          grid-template-rows: repeat(2, 16vw);
          overflow-x: scroll;
          .onlinesrc_item_content_list {
            background: black;
            border-radius: 2vw 2vw 0 0;
            width: 21vw;
            height: 16vw;
            z-index: 20;
            &:hover {
              filter: brightness(150%);
              color: white;
              text-align: center;
            }
            &:active {
              filter: brightness(50%);
              content: attr(data-name);
            }
          }
          .onlinesrc_item_content_text {
            background: rgb(62,181,231);
            border-radius: 0 0 2vw 2vw;
            content: attr(data-name);
            width: 21vw;
            height: 7vw;
            z-index: 21;
          }
        }
      }
    }
    .onlinesrc_layout_share {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 95vw;
      height: 70vh;
      margin: 0.5vh 0 0.5vh 0;
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 30vw;
        height: 15vh;
        font-size: 6vh;
        color: rgb(103, 192, 225);
        line-height: 13vh;
      }
      .onlinesrc_item_background {
        justify-self: center;
        grid-area: content;
        display: flex;
        justify-content: center;
        align-items: center;
        background: rgb(144,217,215);
        width: 100%;
        .onlinesrc_item_content {
          width: 81vw;
          height: 50vh;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: flex-start;
          align-self: flex-end;
          grid-template-columns: repeat(4, 27vw);
          grid-template-rows: repeat(2, 16vw);
          overflow-x: scroll;
          .onlinesrc_item_content_list {
            background: black;
            border-radius: 2vw 2vw 0 0;
            width: 21vw;
            height: 16vw;
            z-index: 20;
            &:hover {
              filter: brightness(150%);
              color: white;
              text-align: center;
            }
            &:active {
              filter: brightness(50%);
              content: attr(data-name);
            }
          }
          .onlinesrc_item_content_text {
            background: rgb(62,181,231);
            border-radius: 0 0 2vw 2vw;
            content: attr(data-name);
            width: 21vw;
            height: 7vw;
            z-index: 21;
          }
        }
      }
    }

    .onlinesrc_back {
      position: absolute;
      z-index: 2;
      right: 2vw;
      top: 3vh;
      z-index: 10;
      .onlinesrc_exit_button {
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
