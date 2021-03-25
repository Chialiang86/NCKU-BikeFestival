<template lang="pug">
  div(class="news_page")
    div(class="news_top_bar_pc")
      div(class="news_top_bar_layout" @click="scroll()")
        router-link(tag="label" class="news_exit_button" to="/")
        div(class="news_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===0")
          label(@click="openTab('https://docs.google.com/forms/d/e/1FAIpQLSfx69xLr9XCqz6y8OEn4d8n6gc4qw3KzOn8FHb7Dm94pGwwmg/viewform'); list = false;" v-if="pc") 我要報名
    div(class="news_top_bar_mobile")
      div(class="news_mobile_title" @click="list = false")
      router-link(tag="div" class="news_mobile_exit_button" to="/")
      div(class="news_mobile_list" @click="list = !list")
    div(class="news_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://docs.google.com/forms/d/e/1FAIpQLSfx69xLr9XCqz6y8OEn4d8n6gc4qw3KzOn8FHb7Dm94pGwwmg/viewform'); list = false;" v-if="!pc") 我要報名
    div(class="news_background")
    div(class="news_logo")
    dive(class="news_flower_top" @click="list = false")
    dive(class="news_flower_down" @click="list = false")
    div(class="news_layout" @click="list = false")
      section(class="news_list_section")
        ul(id="list")
          li(v-for="(iter, index) of news" v-bind:key="iter.Title" v-on:click="currentIndex = currentIndex == -1 ? index : (currentIndex == index ? -1 : index)")
            p {{iter.Title}}
            //time {{parseTimestamp(iter.Timestamp)}}
            time {{iter.Timestamp}}
            div(class="news_content" v-show-slide:300:example-easing="currentIndex === index" v-html="iter.Content")
      //section(class="news_counter_section")
        div(class="news_decoration_top")
        div(class="news_counter")
          div(class="news_countdown_number")
            span(id="news_number_1")
            span(id="news_number_2")
          div(class="news_countdown_dot")
            span(id="news_dot_1")
            span(id="news_dot_2")
            span(id="news_dot_3")
        div(class="news_decoration_bottom")
</template>

<script>
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
      list: false,
      pc: this.isPC(),
      news: [
        {
          Title: '官網正式上線',
          Timestamp: '2021/2/20'
        }
      ],
      currentIndex: -1,
      loader: null
    }
  },
  mounted: async function () {
    this.pc = this.isPC()
    this.setBarHeight()
    setInterval(this.timeCounter, 1000)
    this.numberDom1 = document.querySelector('#news_number_1')
    this.numberDom2 = document.querySelector('#news_number_2')
    this.timeCounter()
    this.loader = this.$loading.show()
    // get news from database
    /*
    try {
      // fetch the articles from database
      const url = `https://us-central1-ncku-bikefestival-12th.cloudfunctions.net/getNewsArticles`
      const result = await axios.get(url)
      this.news = result.data.sort((a, b) => b.Timestamp._seconds - a.Timestamp._seconds)
    } catch (error) {
      console.log(error)
    } */
    this.loader.hide()
  },
  methods: {
    windowSizeChange: function (event) {
      if (window.innerWidth > 599) {
        this.pc = true
      } else {
        this.pc = false
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
    },
    timeCounter: function () {
      const timeStr = '2019/1/26 00:00'
      const startDate = new Date()
      const endDate = new Date(timeStr)
      const spantime = (endDate - startDate) / 1000
      const day = Math.floor(spantime / (24 * 3600))
      if (spantime > 0) {
        let number = require(`../assets/number_${Math.floor(day / 10)}.svg`)
        this.numberDom1.style.setProperty('background-image', `url(${number})`)
        number = require(`../assets/number_${Math.floor(day % 10)}.svg`)
        this.numberDom2.style.setProperty('background-image', `url(${number})`)
      }
    },
    parseTimestamp: function (timestamp) {
      const time = new Date(timestamp._seconds * 1000)
      const str = `${time.getFullYear()}/${time.getMonth() + 1}/${time.getDate()}`
      return str
    }
  }
}
</script>

<style lang="scss" scoped>

  #news_dot_1 {
    animation: dot-loading 1.2s ease-in-out;
    animation-iteration-count: infinite;
  }
  #news_dot_2 {
    animation: dot-loading 1.2s ease-in-out;
    animation-delay: 0.4s;
    animation-iteration-count: infinite;
  }
  #news_dot_3 {
    animation: dot-loading 1.2s ease-in-out;
    animation-delay: 0.8s;
    animation-iteration-count: infinite;
  }

  /*
    mobile layout css
  */
  @media only screen and (max-width: 599px) {
    @keyframes flow-in {
      from { right: -40%; }
      to { right: 0%; }
    }
    .news_page {
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
    .news_background {
      display: none;
    }
    .news_top_bar_mobile {
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
      .news_mobile_exit_button {
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
      .news_mobile_title {
        grid-area: title;
        width: 60vw;
        background-image: url('../assets//news/title.svg');
        background-repeat: no-repeat;
        background-size: 75% 75%;
        background-position: center center;
      }
      .news_mobile_list {
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
    .news_mobile_list_area {
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

    .news_layout {
      display: grid;
      grid-template-rows: 0.8fr 3fr .5fr;
      grid-template-areas: "select"
        "list"
        "list";
      justify-content: center;
      justify-items: center;
      align-items: center;

      width: 100vw;
      height: 100vh;

      .news_list_section {
        grid-area: list;
        justify-self: center;
        align-self: flex-start;

        width: 100%;
        height: 100%;
        overflow-y: auto;
        -webkit-overflow-scrolling: touch;

        &::-webkit-scrollbar {
          width: 6px;
          border-radius: 5px;
        }
        &::-webkit-scrollbar-track {
          background: #f1f1f1;
        }
        &::-webkit-scrollbar-thumb {
          background: #888;
          border-radius: 5px;
        }
        &::-webkit-scrollbar-thumb:hover {
          background: #555;
        }
        ul {
          list-style-type: none;

          margin: 4vw 5vw;
          padding: 0;
          box-sizing: border-box;

          li.news_content {
            padding: 2vh 0 0 0;
          }

          li {
            width: 100%;

            background-color: rgb(86,196,242);

            margin: 0;
            padding: 2vh 4vw;
            border-radius: 2vh;
            box-sizing: border-box;

            line-height: 2.5vh;
            font-size: 2.5vh;
            color: white;
            text-align: left;

            cursor: pointer;
            transition: filter .3s ease;

            &:hover {
              filter: brightness(120%);
            }
            &:active {
              filter: brightness(80%);
            }
            time {
              float: right;
              font-size: 2vh;
            }
            p {
              display: inline-block;
              margin: 0;
            }

            div {
              background-color: transparent;

              line-height: 2vh;
              text-align: left;
              font-size: 2vh;
              color: white;

              transform-origin: top;
              transition: transform .3s ease;
              /deep/ p {
                line-height: 200%;
                font-size: 2vh;
              }
            }

          }
          li+li {
            margin-top: 4vw;
          }
        }
      }
      .news_counter_section {
        display: none;
      }
    }
  }
  /*
    computer layout css
  */
  @media only screen and (min-width: 600px) {
    .news_page {
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
    .news_top_bar_pc {
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
      .news_top_bar_layout {
        display: grid;
        grid-template-columns: 12vw 82vw 6vw;
        grid-template-areas: "home items .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        align-content: center;
        .news_exit_button {
          grid-area: home;
          width: 12vw;
          height: 8vh;
          background-color: transparent;
          background-image: url("../assets//logoHome.svg");
          background-repeat: no-repeat;
          background-size: 80% 80%;
          background-position: 50% 50%;
          background-color: rgb(86,196,242);
          transition: filter .3s ease;
          cursor: pointer;
          &:hover {
            filter: brightness(150%);
          }
          &:active {
            filter: brightness(80%);
          }
        }
        .news_top_bar_item {
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
            color: rgb(86,196,242);
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
        .news_sign_up_button {
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
    .news_background {
      position: absolute;
      z-index: 1;
      top: 0;
      right: -10vw;
      background: rgb(255, 246, 232);
      height: 100vh;
      width: 50vw;
      transform: skewX(5deg);
    }
    .news_logo {
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
    .news_flower_top {
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
    .news_flower_down {
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

    .news_layout {
      display: grid;
      grid-template-columns: 1.5fr 1fr;
      grid-template-rows: 18vh 58vh 20vh;
      grid-template-areas: "empty select"
        "list time"
        ". .";
      justify-content: center;
      justify-items: center;
      align-items: center;

      width: 100vw;
      height: 100vh;

      .news_list_section {
        grid-area: list;
        display: flex;
        justify-self: center;
        align-self: flex-start;
        justify-content: center;

        width: 90%;
        height: 100%;
        margin-top: 5vh;
        max-height: 60vh;
        overflow-y: auto;

        &::-webkit-scrollbar {
          width: 10px;
          border-radius: 5px;
        }
        &::-webkit-scrollbar-track {
          background: #f1f1f1;
        }
        &::-webkit-scrollbar-thumb {
          background: #888;
          border-radius: 5px;
        }
        &::-webkit-scrollbar-thumb:hover {
          background: #555;
        }
        ul {
          list-style-type: none;

          padding: 0 8vw 0 5vw;

          width: 100%;

          li {
            width: 100%;

            background-color: rgb(86,196,242);

            // margin-top: 3vw;
            padding: 2vh 2vw;
            border-radius: 2vh;
            box-sizing: border-box;

            line-height: 2.5vh;
            font-size: 2.5vh;
            color: white;
            text-align: left;

            .news_content {
              padding: 2vh 0vw 0vh 2vw;
            }

            cursor: pointer;
            transition: filter .3s ease;
            overflow-y: hidden;

            &:hover {
              filter: brightness(120%);
            }
            &:active {
              filter: brightness(80%);
            }

            time {
              float: right;
              font-size: 2vh;
            }
            p {
              display: inline-block;
              margin: 0;
            }

            div {
              background-color: transparent;

              text-align: left;
              font-size: 2vh;
              color: white;

              transform-origin: top;
              transition: transform .3s ease;
              /deep/ p {
                line-height: 200%;
                font-size: 2vh;
              }
            }
          }
          li+li {
            margin-top: 2vw;
          }
        }
      }
      .news_counter_section {
        grid-area: time;

        display: grid;
        grid-template-columns: 1fr 2fr 1fr;
        grid-template-rows: 1fr 2fr 1fr;
        grid-template-areas: ". decoration-top ."
          ". counter ."
          ". decoration-bottom .";
        justify-content: center;
        justify-items: center;
        align-items: center;

        width: 100%;

        z-index: 5;

        .news_decoration_top {
          grid-area: decoration-top;
          justify-self: flex-end;

          display: inline-block;
          width: 6vw;
          height: 6vw;
          //background-image: url("../assets/news/news_decoration_top.svg");
          background-repeat: no-repeat;
          background-size: 100% 100%;
        }

        .news_decoration_bottom {
          grid-area: decoration-bottom;
          justify-self: flex-start;

          display: inline-block;
          width: 6vw;
          height: 6vw;
          //background-image: url("../assets/news/news_decoration_bottom.svg");
          background-repeat: no-repeat;
          background-size: 100% 100%;
        }

        .news_counter {
          grid-area: counter;
          justify-self: center;
          align-self: center;

          display: grid;
          grid-template-columns: 1.2fr 1fr;
          grid-template-rows: 1fr 1fr;
          grid-template-areas: "number dot";

          width: 20vw;
          height: 20vw;

          .news_countdown_number {
            grid-area: number;

            display: flex;
            flex-wrap: nowrap;
            justify-content: flex-end;
            align-items: flex-end;
            span {
              display: inline-block;
              width: 46.75%;
              height: 85%;
              background-size: 125% 125%;
              background-position: 50% 50%;
            }
          }

          .news_countdown_dot {
            grid-area: dot;

            display: flex;
            justify-items: flex-end;
            justify-content: flex-end;
            align-items: flex-end;
            span {
              width: 1vw;
              height: 1vw;
              margin: 0.8vw;
              margin-bottom: 5vw;
              background-repeat: no-repeat;
              background-size: 100% 100%;
            }
          }
        }
      }
    }
  }
</style>
