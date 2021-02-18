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
      div(class="activity_mobile_title" @click="list = false; titleBlock = !titleBlock;")
      router-link(tag="div" class="activity_mobile_exit_button" to="/")
      div(class="activity_mobile_list" @click="list = !list; titleBlock = false;")
    div(class="activity_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="!pc") 我要報名
    div(class="activity_flower_top" @click="list = false; titleBlock = false;")
    div(class="activity_flower_down" @click="list = false; titleBlock = false;")
    div(class="activity_top_list_mobile" v-if="titleBlock")
      div(class="title_block" v-for="(title, index) in titleText")
        label(class="title_mobile" v-bind:data-key="'title_'+ `${index+1}`" @click="topicStretch(index);") {{title}}
        div(class="title_topic_mobile" v-bind:data-topic="'topic_'+ `${index+1}`")
          label(v-for="(topic, i) in topicText[index]" @click="selectPageTopic(index, i); titleBlock = false;") - &nbsp {{topic}}
    div(class="activity_layout" @click="list = false; titleBlock = false;")
      div(class="activity_layout_titles" v-if="pc")
        label(v-for="(item, index) in titleText" v-bind:data-key="'title_'+ `${index+1}`" @click="selectPage(index)") {{titleText[index]}}
      div(class="top_title" v-else)
        label {{titleText[currentIndex]}}
        div(class="top_topic")
          label(v-show="currentIndex===0") {{topicText[0][layout1Index]}}
          label(v-show="currentIndex===1") {{topicText[1][layout2Index]}}
          label(v-show="currentIndex===2") {{topicText[2][layout3Index]}}
          label(v-show="currentIndex===3") {{topicText[3][layout4Index]}}
      div(class="activity_layout1" v-show="currentIndex===0")
        div(class="top_bar1" v-if="pc")
          label(v-for="(item, index) in introduceSrc" @click="selectPageTopic(0, index)") {{item.topic}} /
            div(v-if="layout1Index===index")
        div(class="content1")
          div(class="block1" v-for="(element, index) in layout1PageSrc.lists" v-bind:data-block="'block_'+ `${index+1}`" @click="blockStretch(index)")
            img(class="img1" v-bind:src="element.img")
            label(class="title1") {{element.title}}
            div(class="time1")
              label {{element.time}}
            div(class="introduce1")
              p(v-for="text in element.content") {{text}}
      //div(class="activity_layout2"  v-bind:key="currentIndex" v-show="currentIndex===1")
        div(class="top_bar2")
          label(v-for="(item, index) in timeSrc" v-bind:data-day="'day_'+`${index+1}`" @click="selectPageTopic(1, index)") {{item.topic}}
        div(class="content2")
          div(class="block2" v-for="(topic, index) in layout2DaySrc.lists")
            label(class="title2") ➤ &nbsp {{topic.title}}
            table(class="table2" v-bind:id="'table_1_'+`${index+1}`")
              thead
                tr
                  th(v-for="item in topic.table.head") {{item}}
              tbody
                tr(v-for="items in topic.table.body")
                  td(v-for="(item, index) in items" v-bind:id="'td_'+ `${index+1}`")
                    p(v-for="line in item") {{line}}
      div(class="activity_layout3" v-show="currentIndex===2")
      div(class="activity_layout4" v-show="currentIndex===3")
</template>

<script>
import srcJson from '../assets/14/activity/activity.json'
import Waterfall from 'vue-waterfall/lib/waterfall'
import WaterfallSlot from 'vue-waterfall/lib/waterfall-slot'

export default {
  components: {
    Waterfall,
    WaterfallSlot
  },
  mounted: function () {
    for (var i = 0; i < this.introduceSrc.length; i++) {
      var arr = new Array([])
      for (var j = 0; j < this.introduceSrc[i].lists.length; j++) {
        arr.push(false)
      }
      this.layout1FlagArr.push(arr)
    }
    this.pc = this.isPC()
    this.selectPage(0)
  },
  data: function () {
    return {
      currentIndex: -1,
      menuText: ['最新消息', '活動介紹', '科系概覽', '線上資源', '家長專欄', '合作單位', '直播專區'],
      urlText: ['news', 'activity', 'department', 'online', 'parent', 'sponsor', 'live'],
      titleText: ['主題活動介紹', '活動日程', '活動地圖', '認識成大單車節'],
      topicText: [
        ['腳踩單車·夢想啟程', '前進方向·職涯藍圖', '南方躍起·成大風華', '更多精彩活動'],
        ['2/27(六)', '2/28(日)'],
        [],
        []
      ],
      list: false,
      titleBlock: false,
      introduceSrc: srcJson.introduce,
      timeSrc: srcJson.time,
      layout1PageSrc: srcJson.introduce[0],
      layout1Index: 0,
      layout1FlagArr: [],
      layout2DaySrc: srcJson.time[0],
      layout2Index: 0,
      layout3Index: 0,
      layout4Index: 0,
      pc: false
    }
  },
  computed: {
  },
  methods: {
    selectPage: function (index) {
      this.reset(index)
      if (true) {
        if (this.currentIndex === index) {
          this.currentIndex = -1
        } else {
          this.currentIndex = index
          var titleList = document.querySelectorAll('[data-key]')
          for (var i = 0; i < titleList.length; i++) {
            if (i === index) {
              titleList[i].setAttribute('style', `
                background-color: rgb(44,185,244);`)
            } else {
              titleList[i].setAttribute('style', `
                background-color: rgb(117,210,243);`)
            }
          }
        }
      }
    },
    reset: function (index) {
      if (index === 0) {
        this.selectPageTopic(0, 0)
        for (var i = 0; i < this.layout1FlagArr.length; i++) {
          for (var j = 0; j < this.layout1FlagArr[i].length; j++) {
            this.layout1FlagArr[i][j] = false
          }
        }
      }
      if (index === 1) {
        this.selectPageTopic(1, 0)
      }
    },
    selectPageTopic: function (title, index) {
      if (title === 0) {
        this.layout1Index = index
        this.layout1PageSrc = srcJson.introduce[index]
      }
      if (title === 1) {
        this.layout2Index = index
        this.layout2DaySrc = srcJson.time[index]
        var titleList = document.querySelectorAll('[data-day]')
        for (var i = 0; i < titleList.length; i++) {
          if (i === index) {
            titleList[i].setAttribute('style', `
              background-color: rgb(75,196,245);`)
          } else {
            titleList[i].setAttribute('style', `
              background-color: rgb(196, 196, 196);`)
          }
        }
      }
    },
    topicStretch: function (index) {
      var topics = document.querySelectorAll('[data-topic]')
      if (topics[index].offsetHeight === 0) {
        topics[index].setAttribute('style', 'height: auto;')
      } else {
        topics[index].setAttribute('style', 'height: 0;')
      }
    },
    blockStretch: function (index) {
      var block = document.querySelectorAll('[data-block]')
      if (this.layout1FlagArr[this.layout1Index][index]) {
        this.layout1FlagArr[this.layout1Index][index] = false
        block[index].setAttribute('style', 'height: 33vh; transition: filter .9s ease;')
        if (this.pc) {
          block[index].setAttribute('style', 'height: 33vh; transition: filter .9s ease;')
        } else {
          block[index].setAttribute('style', 'height: 22vh; transition: filter .9s ease;')
        }
      } else {
        this.layout1FlagArr[this.layout1Index][index] = true
        block[index].setAttribute('style', 'height: auto; transition: filter .9s ease;')
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
      background: white;
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
    .activity_top_list_mobile {
      position: absolute;
      top: 8vh;
      left: 25vw;
      width: 50vw;
      height: auto;
      max-height: 70vh;
      overflow-y: scroll;
      z-index: 10;

      display: flex;
      flex-direction: column;
      .title_block {
        display: flex;
        flex-direction: column;
        border: 1px solid rgb(255,240,218);
        .title_mobile {
          background-color: rgb(80,196,238);
          height: 6vh;
          color: white;
          height: 6vh;
          line-height: 6vh;
          font-size: 2.6vh;
          font-weight: bold;
          letter-spacing: 0.2vw;
          &:hover {
            filter: brightness(120%);
            transition: .5s ease;
          }
          &:active {
            filter: brightness(60%);
          }
        }
        .title_topic_mobile {
          background-color: rgb(254,241,217);
          height: 0;
          display: flex;
          flex-direction: column;
          label {
            width: 50vw;
            height: 5vh;
            color: rgb(80,196,238);
            line-height: 5vh;
            font-size: 2.2vh;
            letter-spacing: 0.2vw;
            &:hover {
              filter: brightness(120%);
              transition: .5s ease;
            }
            &:active {
              filter: brightness(60%);
            }
          }
        }
      }
    }
    .activity_layout {
      display: grid;
      grid-template-columns: 100vw;
      grid-template-rows: 8vh 10vh 82vh;
      grid-template-areas: "." "topic" "content";
      justify-content: center;
      justify-items: center;
      align-content: center;
      align-items: flex-start;
      width: 100vw;
      height: 100vh;
    }
    .top_title {
      grid-area: topic;
      height: 8vh;
      width: 100vw;
      margin: 1vh 5vw;

      display: flex;
      justify-content: flex-start;
      align-items: center;
      label {
        width: auto;
        margin: 0 2vw 0 5vw;
        padding: 1vh 5vw;
        height: 4vh;
        border-radius: 1.5vh;
        background-color: rgb(75,196,245);
        color: white;
        font-size: 2.5vh;
        font-weight: bold;
        letter-spacing: 0.8vw;
        line-height: 4vh;
      }
      .top_topic {
        height: 5vh;
        width: auto;
        margin: 0;
        padding:0;
        label {
          border-radius: 0%;
          height: 7vh;
          margin: 0 2vw 0 5vw;
          padding: 0;
          background-color: transparent;
          color: rgb(75,196,245);
          font-size: 2vh;
          line-height: 6vh;
          text-align: left;
          background-image: url('../assets/14/activity/text_bottom.png');
          background-repeat: no-repeat;
          background-size: 100% 40%;
          background-position-y: bottom;
        }
      }
    }
    .activity_layout1 {
      grid-area: content;
      overflow-y: scroll;
      overflow-x: hidden;
      height: 80vh;
      .content1 {
        overflow-y: scroll;
        overflow-x: hidden;
        transition: filter .8s ease;
        display: flex;
        flex-direction: column;
        align-items: center;
        .block1 {
          width: 93%;
          height: 22vh;
          background-color: rgb(45,184,245);
          border-radius: 4vw;
          margin: 1vh 3vw;
          box-shadow: 0 0 3px 1px rgba(51, 51, 51, 0.5);

          display: grid;
          grid-template-columns: 3fr 4fr;
          grid-template-rows: 4vh 3vh 1fr;
          grid-template-areas: "img title" "img time" "img introduce";
          &:hover {
            box-shadow: 0.5vw 0.5vh 5px 1px rgba(51, 51, 51, 0.5);
            filter: brightness(110%);
          }
          &:active {
            filter: brightness(50%);
            background-color: rgb(117,210,243);
          }
          .img1 {
            grid-area: img;
            border-radius: 3vw;
            margin: 10% 10%;
            width: 36vw;
            height: 30vw;
            box-shadow: 0 0 3px 2px rgba(51, 51, 51, 0.5);
          }
          .title1 {
            grid-area: title;
            text-align: left;
            margin: 0.2vh 0;
            width: 90%;
            height: 5vh;
            color: white;
            line-height: 6vh;
            font-size: 1.9vh;
            font-weight: bold;
            letter-spacing: 0.2vw;
          }
          .time1 {
            grid-area: time;
            display: flex;
            label {
              border-radius: 3vw;
              margin: 1vh 0 0 0;
              background-color: rgb(255,241,191);
              padding: 0 1vw;
              width: auto;
              height: 2.6vh;
              line-height: 2.6vh;
              font-size: 1.5vh;
              text-align: left;
              color: rgb(153,145,114);
              font-weight: bold;
              letter-spacing: 0.1vw;
            }
          }
          .introduce1 {
            grid-area: introduce;
            margin: 1.5vh 0 3vh 0;
            width: 92%;
            overflow-y: scroll;
              &::-webkit-scrollbar {
              width: 0.2vw;
              border-radius: 0.25vw;
            }
            &::-webkit-scrollbar-track {
              background: transparent;
              border-radius: 0.25vw;
            }
            &::-webkit-scrollbar-thumb {
              background: transparent;
              border-radius: 0.25vw;
            }
            p {
              line-height: 2.2vh;
              font-size: 1.5vh;
              color: white;
              letter-spacing: 0.1vw;
              text-align: left;
            }
          }
        }
      }
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
          transition: filter .8s ease;
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
      right: -12vw;
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
      grid-template-columns: 55vw 35vw;
      grid-template-areas: "page title";
      justify-content: center;
      align-items: center;
      top: 12vh;
      width: 90vw;
      height: 82vh;
      z-index: 2;
    }
    .activity_layout_titles {
      grid-area: title;
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
    .activity_layout1 {
      grid-area: page;
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      overflow-y: scroll;
      overflow-x: hidden;
      .top_bar1 {
        height: 8%;
        width: 100%;
        padding: 0.5vh;
        display: flex;
        align-items: center;
        label {
          width: auto;
          height: 5vh;
          margin: 0 2.5vw 0 0;
          font-size: 2vh;
          color: rgb(61,191,244);
          font-weight: bold;
          letter-spacing: 0.1vh;
          text-align: start;

          display: grid;
          grid-template-rows: 2vh 1vh;
          grid-template-areas: "." "bottom";
          z-index: 5;
          &:hover {
            font-size: 2.1vh;
            filter: brightness(120%);
          }
          &:active {
            filter: brightness(50%);
          }
          div {
            height: 0.5vh;
            grid: bottom;
            background-color: rgb(255,241,191);
            border-radius: 0.25vh;
          }
        }
      }
      .content1 {
        width: 100%;
        height: 90%;
        overflow-y: scroll;
        overflow-x: hidden;
        transition: filter .8s ease;
        &::-webkit-scrollbar {
          width: 0.5vw;
          border-radius: 0.25vw;
        }
        &::-webkit-scrollbar-track {
          background: transparent;
          border-radius: 0.25vw;
        }
        &::-webkit-scrollbar-thumb {
          background: rgb(103, 192, 225);
          border-radius: 0.25vw;
        }
        &::-webkit-scrollbar-thumb:hover {
          filter: brightness(130%);
        }

        display: flex;
        flex-direction: column;
        align-items: flex-start;
        .block1 {
          width: 95%;
          height: 33vh;
          background-color: rgb(45,184,245);
          border-radius: 2vw;
          margin: 1vw 1vw 1vw 0;
          box-shadow: 0 0 3px 1px rgba(51, 51, 51, 0.5);

          display: grid;
          grid-template-columns: 3fr 4fr;
          grid-template-rows: 7vh 5vh 1fr;
          grid-template-areas: "img title" "img time" "img introduce";
          &:hover {
            box-shadow: 0.5vw 0.5vh 5px 1px rgba(51, 51, 51, 0.5);
            filter: brightness(110%);
          }
          &:active {
            filter: brightness(50%);
            background-color: rgb(117,210,243);
          }
          .img1 {
            grid-area: img;
            border-radius: 1vw;
            margin: 8% 12%;
            width: 18vw;
            height: 14vw;
            box-shadow: 0 0 3px 2px rgba(51, 51, 51, 0.5);
          }
          .title1 {
            grid-area: title;
            text-align: left;
            margin: 2.5vh 0 0 0.2vw;
            width: 90%;
            height: 5vh;
            color: white;
            line-height: 6vh;
            font-size: 2.8vh;
            font-weight: bold;
            letter-spacing: 0.2vw;
          }
          .time1 {
            grid-area: time;
            display: flex;
            label {
              border-radius: 1vw;
              margin: 1vh 0 0 0;
              background-color: rgb(255,241,191);
              padding: 0 1vw;
              width: auto;
              height: 3.5vh;
              line-height: 3vh;
              font-size: 2vh;
              text-align: left;
              color: rgb(153,145,114);
              font-weight: bold;
              letter-spacing: 0.1vw;
            }
          }
          .introduce1 {
            grid-area: introduce;
            margin: 0.5vh 0 3vh 0;
            width: 92%;
            overflow-y: scroll;
             &::-webkit-scrollbar {
              width: 0.2vw;
              border-radius: 0.25vw;
            }
            &::-webkit-scrollbar-track {
              background: transparent;
              border-radius: 0.25vw;
            }
            &::-webkit-scrollbar-thumb {
              background: transparent;
              border-radius: 0.25vw;
            }
            p {
              line-height: 3vh;
              font-size: 2vh;
              color: white;
              letter-spacing: 0.1vw;
              text-align: left;
            }
          }
        }
      }
    }
    .activity_layout2 {
      grid-area: page;
      left: 0%;
      top: 0%;
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      overflow-y: scroll;
      z-index: 5;
      overflow-y: scroll;
      .top_bar2 {
        height: 5vh;
        width: auto;
        margin: 2vh 1vw;
        display: flex;
        justify-content: flex-start;
        align-items: flex-start;
        label {
          width: auto;
          height: 3vh;
          line-height: 2.5vh;
          background-color: rgb(196,196,196);
          font-size: 2.5vh;
          font-weight: bold;
          letter-spacing: 0.2vw;
          margin: 0 1vw;
          padding: 1vh 2vw;
          border-radius: 1vh;
          color: white;
          &:hover {
            background-color: rgb(226,226,226);
            font-size: 2.6vh;
          }
          &:active {
            filter: brightness(60%);
            font-size: 2.6vh;
          }
        }
        label[data-day="day_1"] {
          background-color: rgb(75,196,245);
        }
      }
      .content2 {
        width: 100%;
        height: 93%;
        padding: 1vh 1vw;
        overflow-y: scroll;
        .block2 {
          width: 90%;
          height: auto;
          display: grid;
          grid-template-rows: 1fr 5fr;
          grid-template-areas: "title" "table";
          margin: 0 0 3vh 0;
          .title2 {
            grid-area: title;
            text-align: left;
            line-height: 8vh;
            padding-left: 0.5vw;
            color: rgb(75,196,245);
            font-weight: bolder;
            font-size: 3vh;
            letter-spacing: 0.3vw;
          }
          table {
            grid-area: table;
            border-collapse: separate;
            thead {
              background-color: rgb(75,196,245);
              text-align: center;
              vertical-align: middle;
              tr {
                height: 100%;
                th {
                  height: 5vh;
                  padding: 1vh;
                  text-align: center;
                  vertical-align: middle;
                  border: 1px solid rgb(75,196,245);
                  color: white;
                  font-weight: bold;
                  font-size: 2.5vh;
                  letter-spacing: 0.2vw;
                  line-height: 5vh;
                }
                th:first-child {
                  border-top-left-radius: 2vw;
                }
                th:last-child {
                  border-top-right-radius: 2vw;
                }
              }
            }
            tbody {
              tr {
                height: 100%;
                td {
                  width: auto;
                  height: 5vh;
                  padding: 1.5vh 1vw;
                  text-align: center;
                  vertical-align: middle;
                  border: 1px solid rgb(75,196,245);
                  color: rgb(102,102,102);
                  line-height: 3vh;
                  font-size: 2vh;
                }
                #td_1 {
                  text-align: left;
                  min-width: 6vw;
                  max-width: 14vw;
                }
                #td_2 {
                  text-align: left;
                  min-width: 4vw;
                  max-width: 14vw;
                }
                #td_3 {
                  text-align: left;
                  max-width: 10vw;
                }
                #td_4 {
                  text-align: left;
                  min-width: 6vw;
                }
              }
              tr:last-child td:first-child {
                border-bottom-left-radius: 2vw;
              }
              tr:last-child td:last-child {
                border-bottom-right-radius: 2vw;
              }
            }
          }
        }
      }
    }
  }
</style>
