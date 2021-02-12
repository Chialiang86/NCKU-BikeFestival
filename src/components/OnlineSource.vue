<template lang="pug">
  div(class="onlinesrc_page")
    div(class="onlinesrc_title")
    div(class="onlinesrc_flower_top")
    div(class="onlinesrc_flower_down")
    div(class="onlinesrc_layout")
      section(class="onlinesrc_layout_strategy")
        div(class="onlinesrc_layout_strategy_larrow" v-on:click="scroll(-1, 'strategy')")
        div(class="onlinesrc_layout_strategy_rarrow" v-on:click="scroll(1, 'strategy')")
        label(class="onlinesrc_item_name") 備審攻略
        div(class="onlinesrc_item_background")
          div(class="onlinesrc_item_content" id="strategy")
            label(class="onlinesrc_item_content_list" v-for="(iter, index) of contenta" v-bind:key="iter.text" v-bind:data-name="iter.name" v-bind:style="{'background-image': 'url(' + `${iter.img}` + ')'}" v-on:click="openTab(index)")
              label(class="onlinesrc_item_content_text")
                p {{iter.text}}
      section(class="onlinesrc_layout_interview")
        div(class="onlinesrc_layout_interview_larrow" v-on:click="scroll(-1, 'interview')")
        div(class="onlinesrc_layout_interview_rarrow" v-on:click="scroll(1, 'interview')")
        label(class="onlinesrc_item_name") 模擬面試
          p 經驗與技巧分享
        div(class="onlinesrc_item_background")
          div(class="onlinesrc_item_content" id="interview")
            label(class="onlinesrc_item_content_list" v-for="(iter, index) of contenta" v-bind:key="iter.text" v-bind:data-name="iter.name" v-bind:style="{'background-image': 'url(' + `${iter.img}` + ')'}" v-on:click="openTab(index)")
              label(class="onlinesrc_item_content_text")
                p {{iter.text}}
      section(class="onlinesrc_layout_share")
        div(class="onlinesrc_layout_share_larrow" v-on:click="scroll(-1, 'share')")
        div(class="onlinesrc_layout_share_rarrow" v-on:click="scroll(1, 'share')")
        label(class="onlinesrc_item_name") 成大生活分享
        div(class="onlinesrc_item_background")
          div(class="onlinesrc_item_content" id="share")
            label(class="onlinesrc_item_content_list" v-for="(iter, index) of contenta" v-bind:key="iter.text" v-bind:data-name="iter.name" v-bind:style="{'background-image': 'url(' + `${iter.img}` + ')'}" v-on:click="openTab(index)")
              label(class="onlinesrc_item_content_text")
                p {{iter.text}}
      //section(class="onlinesrc_layout_empty")
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
          text: '老高嘴砲喔',
          link: 'https://www.youtube.com/watch?v=5qap5aO4i9A',
          img: 'https://i.imgur.com/Hu5Z3Yq.png'
        },
        {
          text: '背景音樂喔',
          link: 'https://www.youtube.com/watch?v=3mBLjHxwwmo',
          img: 'https://i.imgur.com/jklIThX.png'
        },
        {
          text: '背景音樂喔',
          link: 'https://www.youtube.com/watch?v=3mBLjHxwwmo',
          img: 'https://i.imgur.com/jklIThX.png'
        },
        {
          text: '背景音樂喔',
          link: 'https://www.youtube.com/watch?v=3mBLjHxwwmo',
          img: 'https://i.imgur.com/jklIThX.png'
        },
        {
          text: '背景音樂喔',
          link: 'https://www.youtube.com/watch?v=3mBLjHxwwmo',
          img: 'https://i.imgur.com/jklIThX.png'
        },
        {
          text: '背景音樂喔',
          link: 'https://www.youtube.com/watch?v=3mBLjHxwwmo',
          img: 'https://i.imgur.com/jklIThX.png'
        },
        {
          text: '背景音樂喔',
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
      window.open(this.contenta[index].link, '_blank')
    },
    scroll: function (dir, idName) {
      var scrollPos = document.getElementById(idName)
      const dis = 70
      if (dir === -1) {
        scrollPos.scrollLeft = (scrollPos.scrollLeft - this.vw(dis) < 0) ? 0 : scrollPos.scrollLeft - this.vw(dis)
      }
      if (dir === 1) {
        scrollPos.scrollLeft = (scrollPos.scrollLeft + this.vw(dis) > scrollPos.scrollWidth) ? scrollPos.scrollWidth : scrollPos.scrollLeft + this.vw(dis)
      }
    },
    vw: function (v) {
      var w = Math.max(document.documentElement.clientWidth, window.innerWidth || 0)
      return (v * w) / 100
    },
    vh: function (v) {
      var h = Math.max(document.documentElement.clientHeight, window.innerHeight || 0)
      return (v * h) / 100
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
      top: 3vh;

      width: 45vw;
      height: 20vw;
      background-image: url("../assets/14/live/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: center top 10%;
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
      top: 20vh;
      width: 100vw;
      height: 70vh;
      overflow-y: scroll;
    }

    .onlinesrc_layout_strategy {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 100vw;
      height: 70vh;
      margin: 0 0 2.5vw 0;
      .onlinesrc_layout_strategy_larrow {
        position: absolute;
        width: 10vw;
        height: 12vw;
        left: 3%;
        top: 60%;;
        background-image: url('../assets/14/online/left_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 3px;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 3px;
        }
      }
      .onlinesrc_layout_strategy_rarrow {
        position: absolute;
        width: 10vw;
        height: 12vw;
        right: 3%;
        top: 60%;;
        background-image: url('../assets/14/online/right_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 3px;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 3px;
        }
      }
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 95vw;
        background-image: url('../assets/14/online/pink_cross.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position-y: 55%;
        height: 12vh;
        font-size: 3vh;
        color: rgb(103, 192, 225);
        line-height: 13vh;
        font-weight: bold;
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
          width: 70vw;
          height: 70vw;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(7, 70vw);
          overflow-x: scroll;
          .onlinesrc_item_content_list {
            background: black;
            background-size: contain;
            border-radius: 2vw;
            display: grid;
            grid-template-rows: 2fr 1fr;
            grid-template-areas: "none" "text";
            width: 66vw;
            height: 66vw;
            z-index: 20;
            &:hover {
              filter: brightness(150%);
              width: 68vw;
              height: 68vw;
              border-radius: 2vw;
              color: white;
              text-align: center;
            }
            &:active {
              filter: brightness(50%);
              content: attr(data-name);
            }
            .onlinesrc_item_content_text {
              background: rgb(62,181,231);
              border-radius: 0 0 2vw 2vw;
              content: attr(data-name);
              grid-area: text;
              align-self: flex-end;
              width: 100%;
              height: 100%;
              z-index: 22;
              p {
                padding: 2vh;
                color: rgb(55, 55, 55);
                line-height: 4vh;
                font-size: 4vh;
              }
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
          }
        }
      }
    }

    .onlinesrc_layout_interview {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 100vw;
      height: 70vh;
      margin: 0 0 2.5vw 0;
      .onlinesrc_layout_interview_larrow {
        position: absolute;
        width: 10vw;
        height: 12vw;
        left: 3%;
        top: 160%;;
        background-image: url('../assets/14/online/left_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 3px;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 3px;
        }
      }
      .onlinesrc_layout_interview_rarrow {
        position: absolute;
        width: 10vw;
        height: 12vw;
        right: 3%;
        top: 160%;;
        background-image: url('../assets/14/online/right_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 3px;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 3px;
        }
      }
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 95vw;
        background-image: url('../assets/14/online/yellow_cross.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position-y: 55%;
        height: 12vh;
        font-size: 3vh;
        color: rgb(103, 192, 225);
        line-height: 9vh;
        font-weight: bold;
        p {
          font-size: 2vh;
          color: rgb(103, 192, 225);
          line-height: 0vh;
          font-weight: bold;
        }
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
          width: 70vw;
          height: 70vw;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(7, 70vw);
          overflow-x: scroll;
          .onlinesrc_item_content_list {
            background: black;
            background-size: contain;
            border-radius: 2vw;
            display: grid;
            grid-template-rows: 2fr 1fr;
            grid-template-areas: "none" "text";
            width: 66vw;
            height: 66vw;
            z-index: 20;
            &:hover {
              filter: brightness(150%);
              width: 68vw;
              height: 68vw;
              border-radius: 2vw;
              color: white;
              text-align: center;
            }
            &:active {
              filter: brightness(50%);
              content: attr(data-name);
            }
            .onlinesrc_item_content_text {
              background: rgb(62,181,231);
              border-radius: 0 0 2vw 2vw;
              content: attr(data-name);
              grid-area: text;
              align-self: flex-end;
              width: 100%;
              height: 100%;
              z-index: 22;
              p {
                padding: 2vh;
                color: rgb(55, 55, 55);
                line-height: 4vh;
                font-size: 4vh;
              }
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
          }
        }
      }
    }
    .onlinesrc_layout_share {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 100vw;
      height: 70vh;
      margin: 0 0 2.5vw 0;
      .onlinesrc_layout_share_larrow {
        position: absolute;
        width: 10vw;
        height: 12vw;
        left: 3%;
        top: 260%;;
        background-image: url('../assets/14/online/left_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 3px;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 3px;
        }
      }
      .onlinesrc_layout_share_rarrow {
        position: absolute;
        width: 10vw;
        height: 12vw;
        right: 3%;
        top: 260%;;
        background-image: url('../assets/14/online/right_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 3px;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 3px;
        }
      }
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 95vw;
        background-image: url('../assets/14/online/green_cross.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position-y: 55%;
        height: 12vh;
        font-size: 2.5vh;
        color: rgb(103, 192, 225);
        line-height: 13vh;
        font-weight: bold;
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
          width: 70vw;
          height: 70vw;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(7, 70vw);
          overflow-x: scroll;
          .onlinesrc_item_content_list {
            background: black;
            background-size: contain;
            border-radius: 2vw;
            display: grid;
            grid-template-rows: 2fr 1fr;
            grid-template-areas: "none" "text";
            width: 66vw;
            height: 66vw;
            z-index: 20;
            &:hover {
              filter: brightness(150%);
              width: 68vw;
              height: 68vw;
              border-radius: 2vw;
              color: white;
              text-align: center;
            }
            &:active {
              filter: brightness(50%);
              content: attr(data-name);
            }
            .onlinesrc_item_content_text {
              background: rgb(62,181,231);
              border-radius: 0 0 2vw 2vw;
              content: attr(data-name);
              grid-area: text;
              align-self: flex-end;
              width: 100%;
              height: 100%;
              z-index: 22;
              p {
                padding: 2vh;
                color: rgb(55, 55, 55);
                line-height: 4vh;
                font-size: 4vh;
              }
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
          }
        }
      }
    }
    .onlinesrc_layout_empty {
      width: 100vw;
      height: 5vh;
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
      top: 20vh;
      width: 100vw;
      height: 80vh;
      overflow-y: scroll;
    }

    .onlinesrc_layout_strategy {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 100vw;
      height: 75vh;
      margin: 0 0 2.5vw 0;
      .onlinesrc_layout_strategy_larrow {
        position: absolute;
        width: 5vw;
        height: 6vw;
        left: 6%;
        top: 47%;
        background-image: url('../assets/14/online/left_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 2vh;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 2vh;
        }
      }
      .onlinesrc_layout_strategy_rarrow {
        position: absolute;
        width: 5vw;
        height: 6vw;
        right: 6%;
        top: 47%;
        background-image: url('../assets/14/online/right_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 2vh;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 2vh;
        }
      }
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 90vw;
        background-image: url('../assets/14/online/pink_cross.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position-y: 50%;
        height: 15vh;
        font-size: 6vh;
        color: rgb(103, 192, 225);
        line-height: 13vh;
        font-weight: bold;
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
          width: 78vw;
          height: 26vw;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(7, 26vw);
          overflow-x: scroll;
          .onlinesrc_item_content_list {
            background: black;
            background-size: contain;
            border-radius: 2vw;
            display: grid;
            grid-template-rows: 2fr 1fr;
            grid-template-areas: "none" "text";
            width: 24vw;
            height: 24vw;
            z-index: 20;
            &:hover {
              filter: brightness(150%);
              width: 25vw;
              height: 25vw;
              border-radius: 2vw;
              color: white;
              text-align: center;
            }
            &:active {
              filter: brightness(50%);
              content: attr(data-name);
            }
            .onlinesrc_item_content_text {
              background: rgb(62,181,231);
              border-radius: 0 0 2vw 2vw;
              content: attr(data-name);
              grid-area: text;
              align-self: flex-end;
              width: 100%;
              height: 100%;
              z-index: 22;
              p {
                padding: 2vh;
                color: rgb(55, 55, 55);
                line-height: 4vh;
                font-size: 4vh;
              }
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
          }
        }
      }
    }

    .onlinesrc_layout_interview {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 100vw;
      height: 75vh;
      margin: 0 0 2.5vw 0;
      .onlinesrc_layout_interview_larrow {
        position: absolute;
        width: 5vw;
        height: 6vw;
        left: 6%;
        top: 147%;
        background-image: url('../assets/14/online/left_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 2vh;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 2vh;
        }
      }
      .onlinesrc_layout_interview_rarrow {
        position: absolute;
        width: 5vw;
        height: 6vw;
        right: 6%;
        top: 147%;
        background-image: url('../assets/14/online/right_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 2vh;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 2vh;
        }
      }
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 90vw;
        background-image: url('../assets/14/online/yellow_cross.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position-y: 50%;
        height: 15vh;
        font-size: 6vh;
        color: rgb(103, 192, 225);
        line-height: 5vh;
        font-weight: bold;
        p {
          font-size: 3.6vh;
          color: rgb(103, 192, 225);
          line-height: 8vh;
          font-weight: bold;
        }
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
          width: 78vw;
          height: 26vw;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(7, 26vw);
          overflow-x: scroll;
          .onlinesrc_item_content_list {
            background: black;
            background-size: contain;
            border-radius: 2vw;
            display: grid;
            grid-template-rows: 2fr 1fr;
            grid-template-areas: "none" "text";
            width: 24vw;
            height: 24vw;
            z-index: 20;
            &:hover {
              filter: brightness(150%);
              width: 25vw;
              height: 25vw;
              border-radius: 2vw;
              color: white;
              text-align: center;
            }
            &:active {
              filter: brightness(50%);
              content: attr(data-name);
            }
            .onlinesrc_item_content_text {
              background: rgb(62,181,231);
              border-radius: 0 0 2vw 2vw;
              content: attr(data-name);
              grid-area: text;
              align-self: flex-end;
              width: 100%;
              height: 100%;
              z-index: 22;
              p {
                padding: 2vh;
                color: rgb(55, 55, 55);
                line-height: 4vh;
                font-size: 4vh;
              }
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
          }
        }
      }
    }
    .onlinesrc_layout_share {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 100vw;
      height: 75vh;
      margin: 0 0 2.5vw 0;
      .onlinesrc_layout_share_larrow {
        position: absolute;
        width: 5vw;
        height: 6vw;
        left: 6%;
        top: 247%;
        background-image: url('../assets/14/online/left_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 2vh;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 2vh;
        }
      }
      .onlinesrc_layout_share_rarrow {
        position: absolute;
        width: 5vw;
        height: 6vw;
        right: 6%;
        top: 247%;
        background-image: url('../assets/14/online/right_arrow.svg');
        background-repeat: no-repeat;
        background-size: 65% 65%;
        background-position: center center;
        &:hover {
          background-size: 80% 80%;
          background-color: rgba(100, 100, 100, 0.3);
          border-radius: 2vh;
        }
        &:active {
          background-size: 80% 80%;
          background-color: rgba(200, 200, 200, 0.3);
          border-radius: 2vh;
        }
      }
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 90vw;
        background-image: url('../assets/14/online/green_cross.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position-y: 50%;
        height: 15vh;
        font-size: 6vh;
        color: rgb(103, 192, 225);
        line-height: 13vh;
        font-weight: bold;
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
          width: 78vw;
          height: 26vw;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(7, 26vw);
          overflow-x: scroll;
          .onlinesrc_item_content_list {
            background: black;
            background-size: contain;
            border-radius: 2vw;
            display: grid;
            grid-template-rows: 2fr 1fr;
            grid-template-areas: "none" "text";
            width: 24vw;
            height: 24vw;
            z-index: 20;
            &:hover {
              filter: brightness(150%);
              width: 25vw;
              height: 25vw;
              border-radius: 2vw;
              color: white;
              text-align: center;
            }
            &:active {
              filter: brightness(50%);
              content: attr(data-name);
            }
            .onlinesrc_item_content_text {
              background: rgb(62,181,231);
              border-radius: 0 0 2vw 2vw;
              content: attr(data-name);
              grid-area: text;
              align-self: flex-end;
              width: 100%;
              height: 100%;
              z-index: 22;
              p {
                padding: 2vh;
                color: rgb(55, 55, 55);
                line-height: 4vh;
                font-size: 4vh;
              }
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
          }
        }
      }
    }
    .onlinesrc_layout_empty {
      width: 100vw;
      height: 5vh;
    }

    .onlinesrc_back {
      position: absolute;
      z-index: 2;
      right: 0vw;
      top: 0vh;
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
