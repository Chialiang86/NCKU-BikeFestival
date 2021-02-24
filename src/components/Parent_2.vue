<template lang="pug">
  div(class="parent_page")
    div(class="parent_top_bar_pc")
      div(class="parent_top_bar_layout" @click="scroll()")
        router-link(tag="label" class="parent_exit_button" to="/")
        div(class="parent_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===4")
          label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="pc") 我要報名
    div(class="parent_top_bar_mobile")
      div(class="parent_mobile_title" @click="list = false; titleBlock = false;")
      router-link(tag="div" class="parent_mobile_exit_button" to="/")
      div(class="parent_mobile_list" @click="list = !list")
    div(class="parent_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="!pc") 我要報名
    div(class="parent_background")
    div(class="parent_flower_top" @click="list = false")
    div(class="parent_flower_down" @click="list = false")
    div(class="parent_title_block" v-if="!pc && titleBlock")
      label(v-for="(title, index) in titleText" @click="selectTitle(index); titleBlock = false;") {{title}}
    div(class="parent_layout" @click="list = false")
      div(class="parent_layout_titles" v-if="pc" @click="titleBlock = false;")
        label(v-for="(item, index) in titleText" v-bind:data-key="'title_'+ `${index+1}`" @click="selectTitle(index); titleBlock = false;") {{titleText[index]}}
      div(class="parent_layout_titles_mobile" v-else)
        div(class="current_title" @click="titleBlock = !titleBlock") {{titleText[currentIndex]}}
          div(class="current_title_arrow")
      dic(class="parent_layout_page" @click="titleBlock = false")
        div(class="page_block" v-for="(topics, index) in tempItem" v-bind:data-block="'block_'+`${index}`" @click="blockStretch(index)")
          article(class="page_article")
            p(class="article_title") {{topics.title}}
            div
              p(class="article_content" v-for="(line, i) in topics.content" v-html="line")
            img(class="article_slide" v-for="(name, i) in topics.imgs" v-bind:src="name")
</template>

<script>
import srcJson from '../assets/14/parent/parent.json'
//  import axios from 'axios'
export default {
  created () {
    window.addEventListener('resize', this.windowSizeChange)
  },
  destroyed () {
    window.removeEventListener('resize', this.windowSizeChange)
  },
  data: function () {
    return {
      menuText: ['最新消息', '活動介紹', '科系概覽', '線上資源', '家長專欄', '合作單位', '直播專區'],
      urlText: ['news', 'activity', 'department', 'online', 'parent', 'sponsor', 'live'],
      titleText: ['考試策略', '課綱新制', '院系博覽', '經驗分享', '心態調適', '考前準備', '親子關係', '孩子健康', '未來趨勢', '國際聚焦'],
      list: false,
      titleBlock: false,
      pc: this.isPC(),
      tempItem: srcJson.item[0],
      stretchArr: [],
      currentIndex: 0,
      loader: null
    }
  },
  mounted: function () {
    this.pc = this.isPC()
    // this.setBarHeight()
    // this.loader.hide()
    for (var i = 0; i < srcJson.item.length; i++) {
      var arr = []
      for (var j = 0; j < srcJson.item[i].length; j++) {
        arr.push(false)
      }
      this.stretchArr.push(arr)
    }
  },
  methods: {
    windowSizeChange: function (event) {
      if (window.innerWidth > 551) {
        this.pc = true
      } else {
        this.pc = false
      }
    },
    blockStretch: function (index) {
      var block = document.querySelectorAll('[data-block]')
      if (this.stretchArr[this.currentIndex][index]) {
        this.stretchArr[this.currentIndex][index] = false
        if (this.pc) {
          block[index].setAttribute('style', 'height: 80px; transition: height .9s ease;')
        } else {
          block[index].setAttribute('style', 'height: 10vh; transition: height .9s ease;')
        }
      } else {
        this.stretchArr[this.currentIndex][index] = true
        block[index].setAttribute('style', 'height: auto; transition: height .9s ease;')
      }
    },
    selectTitle: function (index) {
      var titleList = document.querySelectorAll('[data-key]')
      this.currentIndex = index
      for (var i = 0; i < titleList.length; i++) {
        if (i === index) {
          titleList[i].setAttribute('style', `
          background-color: rgb(44,185,244);`)
        } else {
          titleList[i].setAttribute('style', `
          background-color: transparent;`)
          // background-color: rgb(117,210,243);`)
        }
      }
      this.tempItem = srcJson.item[index]
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

  #parent_dot_1 {
    animation: dot-loading 1.2s ease-in-out;
    animation-iteration-count: infinite;
  }
  #parent_dot_2 {
    animation: dot-loading 1.2s ease-in-out;
    animation-delay: 0.4s;
    animation-iteration-count: infinite;
  }
  #parent_dot_3 {
    animation: dot-loading 1.2s ease-in-out;
    animation-delay: 0.8s;
    animation-iteration-count: infinite;
  }

  /*
    mobile layout css
  */
  @media only screen and (max-width: 551px) {
    @keyframes flow-in {
      from { right: -40%; }
      to { right: 0%; }
    }
    .parent_page {
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
      height: auto;
      width: 100vw;
      margin: 0;
      padding: 0;
      background: rgb(255, 246, 232);
    }
    .parent_background {
      display: none;
    }
    .parent_top_bar_mobile {
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
      .parent_mobile_exit_button {
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
      .parent_mobile_title {
        grid-area: title;
        width: 60vw;
        background-image: url('../assets/14/parent/title.svg');
        background-repeat: no-repeat;
        background-size: 75% 75%;
        background-position: center center;
      }
      .parent_mobile_list {
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
    .parent_mobile_list_area {
      position: absolute;
      display: grid;
      grid-template-rows: repeat(8, 7vh);
      z-index: 20;
      width: 40vw;
      height: 92vh;
      right: 0%;
      top: 8%;
      background-color: rgb(250, 242, 226);
      animation: flow-in 0.5s ease;
      label {
        color:rgb(75,196,245);
        line-height: 6vh;
        font-size: 2.4vh;
        letter-spacing: 0.5vw;
        border: 1px solid rgb(200, 200, 200);
        &:hover {
          filter: brightness(150%);
        }
        &:active {
          filter: brightness(60%);
        }
      }
    }

    .parent_title_block {
      position: absolute;
      left: 32vw;
      top: 16.5vh;
      width: calc(36vw + 1px);
      height: auto;
      z-index: 100;

      display: flex;
      flex-direction: column;
      align-items: center;
      label {
        padding: 1.2vh 3vw;
        width: 36vw;
        border: 1px solid rgb(236, 214, 174);
        background-color: rgb(74,194,239);
        color: white;
        height: 2.6vh;
        line-height: 2.6vh;
        font-size: 2vh;
        font-weight: bold;
        letter-spacing: 3px;
        text-align: center;
      }
    }

    .parent_layout {
      display: grid;
      grid-template-rows: 8vh 10vh 1fr;
      grid-template-areas: "." "title" "content";
      justify-content: center;
      justify-items: center;
      align-items: flex-start;

      width: 100vw;
      height: 100vh;
    }

    .parent_layout_titles_mobile {
      grid-area: title;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      z-index: 10;
      .current_title {
        background-color: rgb(74,194,239);
        color: white;
        margin: 2.5vh 0 0 0;
        padding: 1.2vh 3vw;
        border-radius: 10px;
        width: 36vw;
        line-height: 2.5vh;
        font-size: 2.5vh;
        font-weight: bold;
        letter-spacing: 3px;
        text-align: left;
        box-shadow: 1px 1px 2px 1px rgba(51, 51, 51, 0.5);
        z-index: 20;

        display: grid;
        grid-template-columns: 1fr 5vw;
        grid-template-areas: ". arr";
        justify-content: center;
        align-items: flex-end;
        &:hover {
          box-shadow: 0 0 3px 2px rgba(51, 51, 51, 0.5);
        }
        &:active {
          box-shadow: 0 0 2px 1px rgba(51, 51, 51, 0.5);
        }
        .current_title_arrow {
          grid-area: arr;
          width: 4vw;
          height: 4vw;
          transform: rotate(90deg);
          background-image: url('../assets/14/arrow.svg');
          background-size: contain;
          background-repeat: no-repeat;
          background-position: center bottom;
          border-radius: 5px;
        }
      }
    }

    .parent_layout_page {
      grid-area: content;
      width: 100%;
      height: 65vh;

      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: center;
      overflow-y: scroll;
      overflow-x: hidden;
      .page_block {
        height: 10vh;
        width: 94vw;
        margin: 0 2vw 1vh 2vw;
        background-color: rgb(45,184,245);
        border-radius: 10px;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: flex-start;
        &:hover {
          box-shadow: 0.5vw 0.5vh 5px 1px rgba(51, 51, 51, 0.5);
          filter: brightness(110%);
        }
        &:active {
          filter: brightness(50%);
          background-color: rgb(45,184,245);
        }
        .page_article {
          height: auto;
          display: flex;
          flex-direction: column;
          justify-content: flex-start;
          align-items: flex-start;
          overflow: hidden;
          .article_title {
            height: 6vh;
            padding: 2vh 3vw;
            text-align: left;
            line-height: 6vh;
            color: white;
            font-size: 2.4vh;
            letter-spacing: 3px;
          }
          div {
            margin: 1vh 0 0 2vw;
            padding: 3vh 1vw;
            width: 88vw;
            background-color: rgba(255,255,255,0.7);
            .article_content {
              padding: 0 2vw;
              text-align: left;
              line-height: 3vh;
              font-size: calc(8px + 0.7vh);
              color: rgb(100, 100, 100);
            }
          }
          .article_slide {
            margin: 0.5vh 0 0 1vw;
            padding: 0.5vh 1vw;
            width: 90vw;
          }
        }
      }
    }
  }
  /*
    computer layout css
  */
  @media only screen and (min-width: 552px) {
    .parent_page {
      position: absolute;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      width: 100vw;
      min-width: 1000px;
      margin: 0;
      padding: 0;
      background: white;
      overflow: hidden;
    }
    .parent_top_bar_pc {
      position: absolute;
      display: flex;
      justify-content: center;
      min-width: 1000px;
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
      .parent_top_bar_layout {
        display: grid;
        grid-template-columns: 12vw 82vw 6vw;
        grid-template-areas: "home items .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        align-content: center;
        .parent_exit_button {
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
        .parent_top_bar_item {
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
        .parent_sign_up_button {
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
    .parent_background {
      position: absolute;
      z-index: 1;
      top: 0;
      right: -10vw;
      background: rgb(255, 246, 232);
      height: 100vh;
      width: 50vw;
      transform: skewX(5deg);
    }
    .parent_flower_top {
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
    .parent_flower_down {
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
    .parent_layout {
      position: absolute;
      display: grid;
      grid-template-rows: 8vh 80vh;
      grid-template-areas: "title" "page";
      justify-content: center;
      align-items: center;
      top: 12vh;
      width: 90vw;
      height: 82vh;
      z-index: 2;
    }
    .parent_layout_titles {
      grid-area: title;
      display: grid;
      grid-template-columns: repeat(10, 8vw);
    //   grid-template-rows: repeat(5, 10vh);
      align-items: center;
      height: auto;
      min-width: 350px;
    //   margin: 0 1vw;
    //   padding: 3vh 0 0 3vw;
      label {
        width: 8.5vw;
        height: 7vh;
        border-radius: 10px;
        margin: 2vh 0;
        line-height: 6.5vh;
        font-size: 3vh;
        letter-spacing: 0.2vw;
        font-weight: bold;
        color: rgb(44,185,244);
        // background-color: rgb(117,210,243);
        // box-shadow: 0px 0px 2px 1px rgba(0, 0, 0, 0.2);
        &:hover {
          color: white;
          box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
        }
      }
      label[data-key="title_1"] {
        background-color: rgb(44,185,244);
      }
      label[data-key="title_3"] {
        transform: translateX(10px);
      }
      label[data-key="title_4"] {
        transform: translateX(10px);
      }
      label[data-key="title_5"] {
        transform: translateX(20px);
      }
      label[data-key="title_6"] {
        transform: translateX(20px);
      }
      label[data-key="title_7"] {
        transform: translateX(30px);
      }
      label[data-key="title_8"] {
        transform: translateX(30px);
      }
      label[data-key="title_9"] {
        transform: translateX(40px);
      }
      label[data-key="title_10"] {
        transform: translateX(40px);
      }
    }
    .parent_layout_page {
      grid-area: page;
      width: 90vw;
      height: 80vh;
      overflow-x: hidden;
      overflow-y: scroll;
      &::-webkit-scrollbar {
        width: 0.6vw;
        border-radius: 0.5vw;
      }
      &::-webkit-scrollbar-track {
        background: rgba(100, 100, 100, 0.3);
        border-radius: 0.5vw;
      }
      &::-webkit-scrollbar-thumb {
        background: rgb(103, 192, 225);
        border-radius: 0.5vw;
      }
      .page_block{
        width: 85vw;
        max-width: 85vw;
        height: 80px;
        background-color: rgb(45,184,245);
        border-radius: 15px;
        border: 1vw solid transparent;
        margin: 3vh 1vw;
        box-shadow: 0 0 3px 1px rgba(51, 51, 51, 0.5);
        overflow: hidden;

        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: flex-start;
        &:hover {
          box-shadow: 0.5vw 0.5vh 5px 1px rgba(51, 51, 51, 0.5);
          filter: brightness(110%);
        }
        &:active {
          filter: brightness(50%);
          background-color: rgb(45,184,245);
        }
        .page_article {
          padding: 1vh 1vw;
          height: auto;
          display: flex;
          flex-direction: column;
          justify-content: flex-start;
          align-items: flex-start;
          .article_title {
            padding: 1vh 0 2vh 12px;
            text-align: left;
            line-height: 50px;
            color: white;
            font-size: 3vh;
            font-weight: bold;
            letter-spacing: 3px;
          }
          div {
            margin: 1vh 0 5px 12px;
            padding: 3vh 2vw;
            width: 77vw;
            background-color: rgba(255,255,255,0.6);
            .article_content {
              text-align: left;
              line-height: 25px;
              font-size: 16px;
              color: rgb(100, 100, 100);
            }
          }
          .article_slide {
            padding: 5px 12px;
            width: 81vw;
          }
        }
      }
    }
  }
</style>
