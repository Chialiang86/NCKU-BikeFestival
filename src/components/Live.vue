<template lang="pug">
  div(class="live_page")
    div(class="live_top_bar_pc")
      div(class="live_top_bar_layout" @click="scroll()")
        router-link(tag="label" class="live_exit_button" to="/")
        div(class="live_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===6")
          label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="pc") 我要報名
    div(class="live_top_bar_mobile")
      div(class="live_mobile_title" @click="list = false")
      router-link(tag="div" class="live_mobile_exit_button" to="/")
      div(class="live_mobile_list" @click="list = !list")
    div(class="live_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="pc") 我要報名
    div(class="live_dot1_pc")
    div(class="live_dot2_pc")
    div(class="live_dot3_pc")
    div(class="live_layout" @click="list = false")
      div(class="live_layout_yt" @click="list = false")
        iframe(class="live_layout_yt_video" @click="list = false" v-if="ytActive" v-bind:src="ytTemp.link" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen)
        div(class="live_layout_yt_video_none" @click="list = false" v-else)
          lebel(class="live_layout_yt_video_text") 活動當天在此進行直播，詳情請關注臉書或粉絲專頁，謝謝您！
        div(class="live_layout_yt_title" @click="list = false" ) {{ytTemp.title}}
      div(class="live_layout_list" @click="list = false")
        div(class="live_layout_list_content" ref="content" @click="list = false")
          div(class="live_layout_list_block" v-for="items in srcList")
            label(class="live_layout_list_block_title") {{items.title}}
            label(class="live_layout_list_block_list" v-for="(item, index) in items.list" @click="setTemp(items.title + ' | ' + item.name, ytTemp.link=item.link, ytTemp.content=item.content); ytActive=true;") {{item.name}}
        div(class="live_layout_list_logo" @click="list = false")
</template>

<script>
import srcJson from '../assets/14/live/live.json'
export default {
  data: function () {
    return {
      menuText: ['最新消息', '活動介紹', '科系概覽', '線上資源', '家長專欄', '合作單位', '直播專區'],
      urlText: ['news', 'activity', 'department', 'online', 'parent', 'sponsor', 'live'],
      list: false,
      pc: this.isPC(),
      ytTemp: {
        title: '',
        link: '',
        content: ''
      },
      ytActive: false,
      ytClicked: false,
      barHeight: 100,
      srcList: srcJson,
      currentIndex: -1
    }
  },
  mounted: function () {
    this.pc = this.isPC()
    this.setBarHeight()
  },
  methods: {
    openTab: function (url) {
      window.open(url, '_blank')
    },
    setBarHeight: function () {
      this.barHeight = this.$refs.scroll.clientHeight * this.$refs.scroll.clientHeight / this.$refs.content.scrollHeight
    },
    scroll: function () {
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
    setTemp: function (title, link, content) {
      this.ytTemp.title = title
      this.ytTemp.link = link
      this.ytTemp.content = content
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
    .live_top_bar_mobile {
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
      .live_mobile_exit_button {
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
      .live_mobile_title {
        grid-area: title;
        width: 60vw;
        background-image: url('../assets/14/live/title.svg');
        background-repeat: no-repeat;
        background-size: 75% 75%;
        background-position: center center;
      }
      .live_mobile_list {
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
    .live_mobile_list_area {
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
      top: 8vh;
      width: 40vw;
      height: 20vw;
      background-image: url("../assets/14/live/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: center top 10%;
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
      background:radial-gradient(ellipse at center,rgb(230, 230, 230),white);
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
    .live_top_bar_pc {
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
      .live_top_bar_layout {
        display: grid;
        grid-template-columns: 12vw 82vw 6vw;
        grid-template-areas: "home items .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        align-content: center;
        .live_exit_button {
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
        .live_top_bar_item {
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
        .live_sign_up_button {
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
    .live_dot1_pc {
      position: absolute;
      z-index: 2;
      left: 38%;
      top: 4%;
      width: 42vw;
      height: 42vh;
      background-image: url('../assets/14/live/dot1.svg');
      background-repeat: no-repeat;
      background-size: contain;
      background-position: center center;
    }
    .live_dot2_pc {
      position: absolute;
      z-index: 2;
      left: -3%;
      bottom: -2%;
      width: 60vw;
      height: 60vh;
      background-image: url('../assets/14/live/dot2.svg');
      background-repeat: no-repeat;
      background-size: contain;
      background-position: left bottom;
    }
    .live_dot3_pc {
      position: absolute;
      z-index: 2;
      right: -3%;
      bottom: -5%;
      width: 60vw;
      height: 60vh;
      background-image: url('../assets/14/live/dot3.svg');
      background-repeat: no-repeat;
      background-size: contain;
      background-position: right bottom;
    }
    .live_layout {
      position: absolute;
      display: grid;
      justify-content: center;
      grid-template-columns: 5fr 3fr;
      grid-template-areas: "frame list";
      z-index: 2;
      top: 25vh;
      width: 90vw;
      height: 65vh;
    }
    .live_layout_yt {
      z-index: 5;
      grid-area: frame;
      border-radius: 2vw;
      background-color: rgb(64,187,235);
      box-shadow: 0 0 3px 1px rgba(0, 0, 0, 0.5);

      display: grid;
      grid-template-rows: 7fr 1fr;
      grid-template-areas: "video" "title";
      justify-content: center;
      align-items: center;
      .live_layout_yt_video {
        z-index: 6;
        grid-area: video;
        border-radius: 2vw;
        margin: 5vh 3vw 1vh 3vw;
        box-shadow: 0 0 3px 1px rgba(0, 0, 0, 0.5);
        width: 48vw;
        height: 50vh;
      }
      .live_layout_yt_video_none {
        z-index: 6;
        grid-area: video;
        border-radius: 2vw;
        margin: 3vw 3vw 1vh 3vw;
        width: 48vw;
        height: 50vh;
        background-color: rgba(255, 255, 255, 0.5);
        box-shadow: 0 0 3px 1px rgba(0, 0, 0, 0.5);
        display: flex;
        justify-content: center;
        align-items: center;
        .live_layout_yt_video_text {
          z-index: 6;
          width: 30vw;
          height: 10vh;
          line-height: 5vh;
          font-weight: bold;
          font-size: 2.6vh;
          color: white;
        }
      }
      .live_layout_yt_title {
        z-index: 6;
        grid-area: title;
        border-radius: 1vw;
        margin: 0 3vw 1vh 3vw;
        padding: 0 0 0 2vw;
        width: 46vw;
        height: 6vh;
        text-align: left;
        line-height: 6vh;
        font-size: 3vh;
        font-weight: bold;
        color: white;
      }
    }
    .live_layout_list {
      z-index: 6;
      grid-area: list;
      border-radius: 2vw;
      margin: 0 0 0 2vw;
      background-color: rgb(254,241,217);
      box-shadow: 0 0 3px 1px rgba(0, 0, 0, 0.5);

      display: grid;
      grid-template-rows: 6fr 1fr;
      grid-template-areas: "content" "logo";
      .live_layout_list_content {
        z-index: 6;
        grid-area: content;
        max-height: 50vh;
        border-radius: 1vw;
        margin: 5vh 2vw 0 3vw;
        overflow-y: scroll;
        &::-webkit-scrollbar {
          width: 1vw;
          border-radius: 0.5vw;
        }
        &::-webkit-scrollbar-track {
          background: white;
          border-radius: 0.5vw;
          border:rgb(100, 100, 100);
          border: 1px solid rgb(100, 100, 100);
        }
        &::-webkit-scrollbar-thumb {
          background: rgb(103, 192, 225);
          border-radius: 0.5vw;
          border: 1px solid rgb(100, 100, 100);
        }
        &::-webkit-scrollbar-thumb:hover {
          filter: brightness(130%);
        }

        .live_layout_list_block{
          z-index: 6;
          width: 23.5vw;
          height: auto;
          margin: 0 1vh 1vh 1vh;
          display: flex;
          flex-direction: column;
          align-items: flex-end;
          .live_layout_list_block_title {
            z-index: 7;
            width: 21vw;
            height: 6vh;
            margin: 1vh 0 1vh 0;
            padding: 0.5vh 1vw 0.5vh 1.5vw;
            border-radius: 1vw;
            background-color: white;
            text-align: left;
            font-size: 3vh;
            letter-spacing: 0.1vh;
            line-height: 5vh;
            color: rgb(103, 192, 225);
            box-shadow: 0 0 2px 1px rgba(0, 0, 0, 0.5);
          }
          .live_layout_list_block_list {
            z-index: 7;
            width: 20vw;
            height: 5vh;
            margin: 0.5vh 0 0.5vh 1vw;
            padding: 0.5vh 1vw 0.5vh 1vw;
            border-radius: 1vw;
            background-color: rgb(103, 192, 225);
            border: 1px solid rgba(55, 55, 55, 0.3);
            text-align: left;
            font-size: 2.6vh;
            letter-spacing: 0.1vh;
            line-height: 4vh;
            color: white;
            &:hover {
              box-shadow: 0 0 2px 1px rgba(0, 0, 0, 0.5);
              filter: brightness(130%);
            }
            &:active {
              box-shadow: 0 0 2px 1px rgba(0, 0, 0, 0.5);
              filter: brightness(60%);
            }
          }
        }
      }
      .live_layout_list_logo {
        grid-area: logo;
        border-radius: 1vw;
        margin: 1vh 4vw 1vh 4vw;
        background-image: url('../assets/14/live/logo.svg');
        background-repeat: no-repeat;
        background-size: contain;
        background-position: center center;
      }
    }

  }
</style>
