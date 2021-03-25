<template lang="pug">
  div(class="onlinesrc_page")
    div(class="onlinesrc_top_bar_pc")
      div(class="onlinesrc_top_bar_layout")
        router-link(tag="label" class="onlinesrc_exit_button" to="/")
        div(class="onlinesrc_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===3")
    div(class="onlinesrc_top_bar_mobile")
      div(class="onlinesrc_mobile_title" @click="list = false")
      router-link(tag="div" class="onlinesrc_mobile_exit_button" to="/")
      div(class="onlinesrc_mobile_list" @click="list = !list")
    div(class="onlinesrc_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
</template>

<script>
import srcJson from '../assets//online/onlinesrc.json'
export default {
  data: function () {
    return {
      menuText: ['最新消息', '活動介紹', '科系概覽', '線上資源', '家長專欄', '合作單位', '直播專區', '我要報名'],
      urlText: ['news', 'activity', 'department', 'online', 'parent', 'sponsor', 'live', 'sign-up'],
      list: false,
      pc: this.isPC(),
      srcList: srcJson,
      currentIndex: -1
    }
  },
  mounted: async function () {
    this.pc = this.isPC()
  },
  methods: {
    openTab: function (url) {
      window.open(url, '_blank')
    },
    scroll: function (dir, idName) {
      var scrollPos = document.getElementById(idName)
      const dis = this.pc ? 26 : 70
      if (dir === -1) {
        scrollPos.scrollLeft = (scrollPos.scrollLeft - this.vw(dis) < 0) ? 0 : scrollPos.scrollLeft - this.vw(dis)
      }
      if (dir === 1) {
        scrollPos.scrollLeft = (scrollPos.scrollLeft + this.vw(dis) > scrollPos.scrollWidth) ? scrollPos.scrollWidth : scrollPos.scrollLeft + this.vw(dis)
      }
    },
    isPC: function () {
      var userAgentInfo = navigator.userAgent
      var Agents = ['Android', 'iPhone', 'SymbianOS', 'Windows Phone', 'iPad', 'iPod']
      var flag = true
      for (var v = 0; v < Agents.length; v++) {
        if (userAgentInfo.indexOf(Agents[v]) > 0) {
          flag = false
          break
        }
      }
      return flag
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
    .onlinesrc_top_bar_mobile {
      position: absolute;
      display: grid;
      grid-template-columns: 20vw 1fr 20vw;
      grid-template-areas: "exit title list";
      justify-content: center;
      justify-items: center;
      z-index: 10;
      top: 0%;
      left: 0%;
      background-color: rgb(254,241,217);
      width: 100vw;
      height: 8vh;
      box-shadow: 0 0 3px 1px rgba(51, 51, 51, 0.5);
      &:hover {
        box-shadow: 0 0 4px 2px rgba(51, 51, 51, 0.5);
      }
      .onlinesrc_mobile_exit_button {
        grid-area: exit;
        width: 6vh;
        height: 6vh;
        background-image: url('../assets//exit.svg');
        background-repeat: no-repeat;
        background-size: 60% 60%;
        background-position: center center;
        border-radius: 2vw;
        background-color: transparent;
        margin: 1vh;
        &:hover {
          background-color: rgba(155, 155, 155, 0.8);
          filter: brightness(150%);
        }
        &:active {
          background-color: rgba(155, 155, 155, 0.8);
          filter: brightness(60%);
        }
      }
      .onlinesrc_mobile_title {
        grid-area: title;
        width: 60vw;
        background-image: url('../assets//online/title.svg');
        background-repeat: no-repeat;
        background-size: 75% 75%;
        background-position: center center;
      }
      .onlinesrc_mobile_list {
        grid-area: list;
        width: 6vh;
        height: 6vh;
        background-image: url('../assets//list.svg');
        background-repeat: no-repeat;
        background-size: 60% 60%;
        background-position: center center;
        border-radius: 2vw;
        background-color: transparent;
        margin: 1vh;
        &:hover {
          background-color: rgba(155, 155, 155, 0.8);
          filter: brightness(150%);
        }
        &:active {
          background-color: rgba(155, 155, 155, 0.8);
          filter: brightness(60%);
        }
      }
    }
    .onlinesrc_mobile_list_area {
      position: absolute;
      display: grid;
      grid-template-rows: repeat(8, 7vh);
      z-index: 20;
      width: 40vw;
      height: 56vh;
      right: 0%;
      top: 8%;
      background-color: rgba(100, 100, 100, 0.9);
      label {
        border: 0.1vh solid black;
        color:rgb(255, 246, 232);
        line-height: 6vh;
        font-size: 2.4vh;
        &:hover {
          background-color: rgb(155, 155, 155);
          filter: brightness(150%);
        }
        &:active {
          background-color: rgba(155, 155, 155, 0.8);
          filter: brightness(60%);
        }
      }
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
      top: 8vh;
      width: 40vw;
      height: 20vw;
      background-image: url("../assets//live/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: center top 10%;
    }
    .onlinesrc_logo {
      position: absolute;
      background-image: url('../assets//pageLogo.svg');
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
      background-image: url('../assets//flower.svg');
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
      background-image: url('../assets//flower.svg');
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
      top: 8vh;
      width: 100vw;
      height: 92vh;
      overflow-y: scroll;
      scroll-behavior: smooth;
    }

    .onlinesrc_layout_strategy {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 100vw;
      height: 70vh;
      margin: 0 0 2.5vw 0;
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 95vw;
        background-image: url('../assets//online/pink_cross.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position-y: 55%;
        height: 12vh;
        font-size: 3.6vh;
        color: rgb(103, 192, 225);
        line-height: 13vh;
        font-weight: bold;
      }
      .onlinesrc_item_background {
        justify-self: center;
        grid-area: content;
        display: grid;
        grid-template-columns: 3vw 10vw 2vw 1fr 2vw 10vw 3vw;
        grid-template-areas: ". la . content . ra .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        background: rgb(241,207,217);
        width: 100%;
        .onlinesrc_layout_strategy_larrow {
          grid-area: la;
          width: 10vw;
          height: 12vw;
          background-image: url('../assets//online/left_arrow.svg');
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
          grid-area: ra;
          width: 10vw;
          height: 12vw;
          background-image: url('../assets//online/right_arrow.svg');
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
        .onlinesrc_item_content {
          width: 70vw;
          height: 70vw;
          grid-area: content;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(32, 70vw);
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
                color: white;
                line-height: 4vh;
                font-size: 3vh;
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
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 95vw;
        background-image: url('../assets//online/yellow_cross.svg');
        background-size: contain;
        background-repeat: no-repeat;
        background-position-y: 55%;
        height: 12vh;
        font-size: 3.6vh;
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
        display: grid;
        grid-template-columns: 3vw 10vw 2vw 1fr 2vw 10vw 3vw;
        grid-template-areas: ". la . content . ra .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        background: rgb(254,241,217);
        width: 100%;
        .onlinesrc_layout_interview_larrow {
          grid-area: la;
          width: 10vw;
          height: 12vw;
          background-image: url('../assets//online/left_arrow.svg');
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
          grid-area: ra;
          width: 10vw;
          height: 12vw;
          background-image: url('../assets//online/right_arrow.svg');
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
        .onlinesrc_item_content {
          width: 70vw;
          height: 70vw;
          grid-area: content;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(32, 70vw);
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
                color: white;
                line-height: 4vh;
                font-size: 3vh;
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
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 95vw;
        background-image: url('../assets//online/green_cross.svg');
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
        display: grid;
        grid-template-columns: 3vw 10vw 2vw 1fr 2vw 10vw 3vw;
        grid-template-areas: ". la . content . ra .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        background: rgb(144,217,215);
        width: 100%;
        .onlinesrc_layout_share_larrow {
          grid-area: la;
          width: 10vw;
          height: 12vw;
          background-image: url('../assets//online/left_arrow.svg');
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
          grid-area: ra;
          width: 10vw;
          height: 12vw;
          background-image: url('../assets//online/right_arrow.svg');
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
        .onlinesrc_item_content {
          width: 70vw;
          height: 70vw;
          grid-area: content;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(32, 70vw);
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
                color: white;
                line-height: 4vh;
                font-size: 3vh;
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
    .onlinesrc_layout_empty_down {
      width: 100vw;
      height: 20vh;
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
        background-image: url("../assets//home.svg");
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
    // .onlinesrc_title {
    //   position: absolute;
    //   z-index: 20;
    //   left: 4vw;
    //   top: 6vh;

    //   width: 17vw;
    //   height: 8vw;
    //   background-image: url("../assets//online/title.svg");
    //   background-repeat: no-repeat;
    //   background-size: 100% 100%;
    //   background-position: 50% 50%;
    // }
    .onlinesrc_top_bar_pc {
      position: absolute;
      display: flex;
      justify-content: center;
      z-index: 100;
      top: 0%;
      left: 0%;
      background-color: rgb(254,241,217);
      width: 100vw;
      height: 8vh;
      box-shadow: 0 0 3px 1px rgba(51, 51, 51, 0.5);
      &:hover {
        box-shadow: 0 0 4px 2px rgba(51, 51, 51, 0.5);
      }
      .onlinesrc_top_bar_layout {
        display: grid;
        grid-template-columns: 12vw 82vw 6vw;
        grid-template-areas: "home items .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        align-content: center;
        .onlinesrc_exit_button {
          grid-area: home;
          width: 12vw;
          height: 8vh;
          background-color: transparent;
          background-image: url("../assets//logoHome.svg");
          background-repeat: no-repeat;
          background-size: 80% 80%;
          background-position: 50% 50%;
          background-color: rgb(103, 192, 225);
          transition: filter .3s ease;
          cursor: pointer;
          &:hover {
            filter: brightness(150%);
          }
          &:active {
            filter: brightness(80%);
          }
        }
        .onlinesrc_top_bar_item {
          grid-area: items;
          display: grid;
          grid-template-columns: repeat(8, 10vw);
          justify-content: center;
          label {
            display: grid;
            grid-template-rows: 5fr 1fr;
            grid-template-areas: "." "bottom";
            width: 9vw;
            height: 6vh;
            line-height: 5.6vh;
            font-size: 2.5vh;
            font-weight: 700;
            background-color: transparent;
            color: rgb(103, 192, 225);
            letter-spacing: 0.2vw;
            #bottom {
              grid-area: "bottom";
              background-color: white;
            }
            &:hover {
              filter: brightness(150%);
              background-color: rgba(55, 55, 55, 0.3);
            }
            &:active {
              filter: brightness(80%);
            }
          }
        }
        .onlinesrc_sign_up_button {
          grid-area: sign-up;
          width: 8vw;
          height: 6vh;
          line-height: 5vh;
          font-size: 3vh;
          background-color: white;
          border: 1px solid rgba(100, 100, 100, 0.3)
        }
      }
    }
    .onlinesrc_logo {
      position: absolute;
      background-image: url('../assets//pageLogo.svg');
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
      background-image: url('../assets//flower.svg');
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
      background-image: url('../assets//flower.svg');
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
      top: 8vh;
      width: 100vw;
      height: 92vh;
      overflow-y: scroll;
    }

    .onlinesrc_layout_strategy {
      display: grid;
      grid-template-rows: 1fr 4fr;
      grid-template-areas: "name" "content";
      width: 100vw;
      height: 75vh;
      margin: 0 0 5vw 0;
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 90vw;
        background-image: url('../assets//online/pink_cross.svg');
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
        display: grid;
        grid-template-columns: 4vw 5vw 1vw 1fr 1vw 5vw 4vw;
        grid-template-areas: ". la . content . ra .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        background: rgb(241,207,217);
        width: 100%;
        .onlinesrc_layout_strategy_larrow {
          grid-area: la;
          width: 5vw;
          height: 6vw;
          background-image: url('../assets//online/left_arrow.svg');
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
          grid-area: ra;
          width: 5vw;
          height: 6vw;
          background-image: url('../assets//online/right_arrow.svg');
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
        .onlinesrc_item_content {
          width: 78vw;
          height: 26vw;
          grid-area: content;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(32, 26vw);
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
                color: white;
                line-height: 4vh;
                font-size: 3vh;
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
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 90vw;
        background-image: url('../assets//online/yellow_cross.svg');
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
        display: grid;
        grid-template-columns: 4vw 5vw 1vw 1fr 1vw 5vw 4vw;
        grid-template-areas: ". la . content . ra .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        background: rgb(254,241,217);
        width: 100%;
        .onlinesrc_layout_interview_larrow {
          grid-area: la;
          width: 5vw;
          height: 6vw;
          background-image: url('../assets//online/left_arrow.svg');
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
          grid-area: ra;
          width: 5vw;
          height: 6vw;
          background-image: url('../assets//online/right_arrow.svg');
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
        .onlinesrc_item_content {
          width: 78vw;
          height: 26vw;
          grid-area: content;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(32, 26vw);
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
                color: white;
                line-height: 4vh;
                font-size: 3vh;
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
      .onlinesrc_item_name {
        justify-self: center;
        grid-area: name;
        width: 90vw;
        background-image: url('../assets//online/green_cross.svg');
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
        display: grid;
        grid-template-columns: 4vw 5vw 1vw 1fr 1vw 5vw 4vw;
        grid-template-areas: ". la . content . ra .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        background: rgb(144,217,215);
        width: 100%;
        .onlinesrc_layout_share_larrow {
          grid-area: la;
          width: 5vw;
          height: 6vw;
          background-image: url('../assets//online/left_arrow.svg');
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
          grid-area: ra;
          width: 5vw;
          height: 6vw;
          background-image: url('../assets//online/right_arrow.svg');
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
        .onlinesrc_item_content {
          width: 78vw;
          height: 26vw;
          grid-area: content;
          display: grid;
          justify-content: flex-start;
          justify-items: center;
          align-items: center;
          align-self: center;
          grid-template-columns: repeat(32, 26vw);
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
                color: white;
                line-height: 4vh;
                font-size: 3vh;
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
    .onlinesrc_layout {
      position: absolute;
      z-index: 3;
      top: 8vh;
      width: 100vw;
      height: 92vh;
    }
  }
</style>
