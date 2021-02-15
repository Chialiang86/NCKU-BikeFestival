<template lang="pug">
  div(class="activity_page")
    div(class="activity_top_bar_pc")
      div(class="activity_top_bar_layout" @click="scroll()")
        router-link(tag="label" class="activity_exit_button" to="/")
        div(class="activity_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===1")
          label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="pc") 我要報名
    div(class="activity_top_bar_mobile")
      div(class="activity_mobile_title" @click="list = false")
      router-link(tag="div" class="activity_mobile_exit_button" to="/")
      div(class="activity_mobile_list" @click="list = !list")
    div(class="activity_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="pc") 我要報名
    div(class="activity_background")
    div(class="activity_flower_top" @click="list = false")
    div(class="activity_flower_down" @click="list = false")
    div(class="activity_layout" @click="list = false")
      div(class="activity_layout_titles")
        label(v-for="(item, index) in titleText" v-bind:data-key="'title_'+ `${index+1}`" @click="selectPage(index + 1)") {{titleText[index]}}
      div(class="activity_layout_page_1" v-if="currentIndex===1")
      div(class="activity_layout_page_2" v-if="currentIndex===2")
      div(class="activity_layout_page_3" v-if="currentIndex===3")
      div(class="activity_layout_page_4" v-if="currentIndex===4")
</template>

<script>
import Waterfall from 'vue-waterfall/lib/waterfall'
import WaterfallSlot from 'vue-waterfall/lib/waterfall-slot'

export default {
  components: {
    Waterfall,
    WaterfallSlot
  },
  mounted: function () {
    this.pc = this.isPC()
  },
  data: function () {
    return {
      currentIndex: -1,
      menuText: ['最新消息', '活動介紹', '科系概覽', '線上資源', '家長專欄', '合作單位', '直播專區'],
      urlText: ['news', 'activity', 'department', 'online', 'parent', 'sponsor', 'live'],
      titleText: ['主題活動介紹', '活動日程', '活動地圖', '認識成大單車節'],
      list: false,
      pc: this.isPC()
    }
  },
  methods: {
    selectPage: function (index) {
      if (this.currentIndex === index) {
        this.currentIndex = 0
      } else {
        this.currentIndex = index
        var titleList = document.querySelectorAll('[data-key]')
        for (var i = 0; i < titleList.length; i++) {
          if (i === index - 1) {
            titleList[i].setAttribute('style', `
              background-color: rgb(44,185,244);`)
          } else {
            titleList[i].setAttribute('style', `
              background-color: rgb(117,210,243);`)
          }
        }
      }
    },
    openTab: function (url) {
      window.open(url, '_blank')
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
    }
  }
}
</script>

<style lang="scss" scoped>

  /*
    mobile layout css
  */
  @media only screen and (max-width: 551px) {
    @font-face {
    font-family: 'GenYoGothicTW-Bold';
    src: url('../assets/fonts/GenYoGothicTW-Bold.woff') format("woff"),
          url('../assets/fonts/GenYoGothicTW-Bold.ttf') format("truetype"),
          url('../assets/fonts/GenYoGothicTW-Bold.eot') format("embedded-opentype");
    }
    @import url('https://fonts.googleapis.com/css?family=Noto+Sans+TC');
    *{
      font-family: 'Noto Sans TC'!important;
    }
    .activity_page {
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
      width: 100vw;
      margin: 0;
      padding: 0;
      background: rgb(255, 246, 232);
    }
    .activity_top_bar_mobile {
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
      .activity_mobile_exit_button {
        grid-area: exit;
        width: 6vh;
        height: 6vh;
        background-image: url('../assets/14/exit.svg');
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
      .activity_mobile_title {
        grid-area: title;
        width: 60vw;
        background-image: url('../assets/14/activity/title.svg');
        background-repeat: no-repeat;
        background-size: 75% 75%;
        background-position: center center;
      }
      .activity_mobile_list {
        grid-area: list;
        width: 6vh;
        height: 6vh;
        background-image: url('../assets/14/list.svg');
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
    .activity_mobile_list_area {
      position: absolute;
      display: grid;
      grid-template-rows: repeat(8, 7vh);
      z-index: 100;
      width: 40vw;
      height: 56vh;
      right: 0%;
      top: 8%;
      background-color: rgba(100, 100, 100, 0.9);
      label {
        color:rgb(255, 246, 232);
        line-height: 6vh;
        font-size: 2.4vh;
        border: 1px solid rgb(50, 50, 50);
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
    .activity_background {
      display: none;
    }
    .activity_layout {
      display: grid;
      grid-template-columns: 100vw;
      grid-template-rows: 25vh 3.5fr;
      grid-template-areas: "select"
        "list";
      justify-content: center;
      justify-items: center;
      align-content: center;
      align-items: center;
      width: 100vw;
      min-height: 100vh;
    }
  }
  /*
    computer layout css
  */
  @media only screen and (min-width: 552px) {
    @font-face {
    font-family: 'GenYoGothicTW-Bold';
    src: url('../assets/fonts/GenYoGothicTW-Bold.woff') format("woff"),
          url('../assets/fonts/GenYoGothicTW-Bold.ttf') format("truetype"),
          url('../assets/fonts/GenYoGothicTW-Bold.eot') format("embedded-opentype");
    }
    @import url('https://fonts.googleapis.com/css?family=Noto+Sans+TC');
    *{
      font-family: 'Noto Sans TC'!important;
    }
    .activity_page {
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
    .activity_top_bar_pc {
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
      .activity_top_bar_layout {
        display: grid;
        grid-template-columns: 12vw 82vw 6vw;
        grid-template-areas: "home items .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        align-content: center;
        .activity_exit_button {
          grid-area: home;
          width: 12vw;
          height: 8vh;
          background-color: transparent;
          background-image: url("../assets/14/logoHome.svg");
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
        .activity_top_bar_item {
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
        .activity_sign_up_button {
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
    .activity_background {
      position: absolute;
      z-index: 1;
      top: 0;
      right: -10vw;
      background: rgb(255, 246, 232);
      height: 100vh;
      width: 50vw;
      transform: skewX(5deg);
    }
    .activity_flower_top {
      position: absolute;
      background-image: url('../assets/14/flower.svg');
      background-repeat: no-repeat;
      background-position: center top;
      background-size: cover;
      z-index: 2;
      right: 0vw;
      top: 5vh;
      width: 41vw;
      height: 40vh;
    }
    .activity_flower_down {
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
    .activity_layout {
      position: absolute;
      display: grid;
      grid-template-columns: 58vw 32vw;
      grid-template-areas: "page title";
      justify-content: center;
      align-items: center;
      // border: 1px solid black;
      top: 10vh;
      width: 90vw;
      height: 85vh;
      z-index: 2;
    }
    .activity_layout_titles {
      grid-area: title;
      // border: 1px solid black;
      display: flex;
      flex-direction: column;
      align-items: center;
      height: auto;
      margin: 1vw;
      padding: 5vh 0 0 0;
      label {
        width: 25vw;
        height: 8vh;
        border-radius: 2vh;
        margin: 2vh 0;
        line-height: 7vh;
        font-size: 3vh;
        letter-spacing: 0.2vw;
        font-weight: bold;
        color: white;
        background-color: rgb(117,210,243);
        box-shadow: 0px 0px 2px 1px rgba(0, 0, 0, 0.2);
        &:hover {
          background-color: rgb(44,185,244);
          box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
        }
      }
      label[data-key="title_2"] {
        transform: translateX(1vw);
      }
      label[data-key="title_3"] {
        transform: translateX(2vw);
      }
      label[data-key="title_4"] {
        transform: translateX(3vw);
      }
    }
  }
</style>
