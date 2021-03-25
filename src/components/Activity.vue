<template lang="pug">
  div(class="activity_page")
    div(class="activity_background")
    div(class="live_dot2")
    div(class="live_dot3")
    div(class="live_dot4")
    div(class="activity_top_bar_pc")
      div(class="activity_top_bar_layout" @click="scroll()")
        router-link(tag="label" class="activity_exit_button" to="/")
        div(class="activity_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===1")
          label(@click="openTab('https://docs.google.com/forms/d/e/1FAIpQLSfx69xLr9XCqz6y8OEn4d8n6gc4qw3KzOn8FHb7Dm94pGwwmg/viewform'); list = false;" v-if="pc") 我要報名
    div(class="activity_top_bar_mobile")
      div(class="activity_mobile_title" @click="list = false;")
      router-link(tag="div" class="activity_mobile_exit_button" to="/")
      div(class="activity_mobile_list" @click="list = !list; titleBlock = false;")
    div(class="activity_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://docs.google.com/forms/d/e/1FAIpQLSfx69xLr9XCqz6y8OEn4d8n6gc4qw3KzOn8FHb7Dm94pGwwmg/viewform'); list = false;" v-if="!pc") 我要報名
    div(class="activity_flower_top" @click="list = false; titleBlock = false;")
    div(class="activity_flower_down" @click="list = false; titleBlock = false;")
    div(class="activity_topic_list_mobile" v-if="titleBlock")
      div(class="title_block" v-for="(title, index) in titleText")
        label(class="title_mobile" v-bind:data-key="'title_'+ `${index+1}`" @click="selectPageTopic(index, 0); titleBlock = false;") {{title}}
        div(class="title_topic_mobile" v-bind:data-topic="'topic_'+ `${index+1}`")
          label(v-for="(topic, i) in topicText[index]" @click="selectPageTopic(index, i); titleBlock = false;") {{topic}}
    div(class="activity_layout" @click="list = false;")
      div(class="activity_layout_titles" v-if="pc" @click="titleBlock = false;")
        label(v-for="(item, index) in titleText" v-bind:data-key="'title_'+ `${index+1}`" @click="selectPageTopic(index, 0); titleBlock = false;") {{titleText[index]}}
      div(class="top_title" v-else )
        div(class="top_title_first" @click="titleBlock = !titleBlock;" onchange="") {{titleText[currentIndex]}}
          label(class="top_title_arrow")
        div(class="top_topic" @click="titleBlock = false;")
          label(v-show="currentIndex===0") {{topicText[0][layout1Index]}}
          label(v-show="currentIndex===1") {{topicText[1][layout2Index]}}
          label(v-show="currentIndex===2") {{topicText[2][layout3Index]}}
          label(v-show="currentIndex===3") {{topicText[3][layout4Index]}}
      div(class="activity_layout1" v-show="currentIndex===0")
        div(class="top_bar1" v-show="pc")
          label(v-for="(item, index) in introduceSrc" @click="selectPageTopic(0, index)") {{item.topic}} /
            div(v-if="layout1Index===index")
        div(class="content1" @click="titleBlock = false;")
          div(class="block1" v-for="(element, index) in layout1PageSrc.lists" v-model="layout1PageSrc" v-bind:data-block="'block_'+ `${index+1}`" @click="blockStretch(index)")
            img(class="img1" v-bind:src="element.img")
            label(class="title1") {{element.title}}
            div(class="time1")
              label {{element.time}}
            div(class="introduce1")
              p(v-for="text in element.content") {{text}}
          div(class="block1_empty")
      div(class="activity_layout2"  v-bind:key="currentIndex" v-show="currentIndex===1")
        div(class="top_bar2" v-show="pc" @click="titleBlock = false;")
          label(v-for="(item, index) in timeSrc" v-bind:data-day="'day_'+`${index+1}`" @click="selectPageTopic(1, index)") {{item.topic}}
        div(class="content2" @click="titleBlock = false;")
          div(class="block2" v-for="(topic, index) in layout2DaySrc.lists")
            label(class="title2") ➤ &nbsp {{topic.title}}
            table(class="table2" v-bind:data-table="'table_'+`${layout2Index+1}`+'_'+`${index+1}`")
              thead
                tr
                  th(v-for="item in topic.table.head") {{item}}
              tbody
                tr(v-for="items in topic.table.body")
                  td(v-for="(item, index) in items" v-bind:data-td="'td_'+ `${index+1}`")
                    p(v-for="line in item") {{line}}
          div(class="content2_empty")
      div(class="activity_layout3" v-show="currentIndex===2" @click="list = false; titleBlock = false;")
        dic(class="map_block")
          img(class="map_img" v-bind:src="layout3MapSrc" @click="layout3Index = !layout3Index;")
          label(class="map_msg" v-show="pc")
        label(class="map_big_msg" v-show="!pc") 點圖可放大
      div(class="activity_layout4" v-show="currentIndex===3" @click="list = false; titleBlock = false;")
        div(class="block4")
          img(class="img4" v-bind:src="layout4AboutSrc.img")
          article(class="article4" id="article")
    div(class="activity_big_map" v-if="layout3Index" @click="layout3Index = !layout3Index;")
      img(v-bind:src="layout3MapSrc")
</template>

<script>
import srcJson from '../assets//activity/activity.json'
import Waterfall from 'vue-waterfall/lib/waterfall'
import WaterfallSlot from 'vue-waterfall/lib/waterfall-slot'

export default {
  components: {
    Waterfall,
    WaterfallSlot
  },
  created () {
    window.addEventListener('resize', this.windowSizeChange)
  },
  destroyed () {
    window.removeEventListener('resize', this.windowSizeChange)
  },
  mounted: function () {
    var mapElement = document.getElementsByClassName('map_img')[0]
    var articleContent = document.getElementsByClassName('article4')[0]
    mapElement.addEventListener('mousemove', this.mapMaxMsg)
    mapElement.addEventListener('mouseleave', this.mapMaxMsgOut)
    articleContent.innerHTML = this.layout4Article
    for (var i = 0; i < this.introduceSrc.length; i++) {
      var arr = new Array([])
      for (var j = 0; j < this.introduceSrc[i].lists.length; j++) {
        arr.push(false)
      }
      this.layout1FlagArr.push(arr)
    }
    this.pc = this.isPC()
    this.selectPageTopic(0, 0)
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
      titleFlag: false,
      introduceSrc: srcJson.introduce,
      timeSrc: srcJson.time,
      layout1PageSrc: srcJson.introduce[0],
      layout1Index: 0,
      layout1FlagArr: [],
      layout2DaySrc: srcJson.time[0],
      layout2Index: 0,
      layout3MapSrc: srcJson.map,
      layout3Index: false,
      layout4Index: 0,
      layout4AboutSrc: srcJson.about,
      layout4Article: ` <p>&nbsp &nbsp &nbsp &nbsp 國立成功大學為全台首屈一指的綜合性大學，為各專業領域培育兼具實力及視野的菁英人才，除學術研究外，產學合作上均對本國有舉足輕重的影響地位。乘載著台灣歷史中的種種足跡，成大一再尋求蛻變，以「教育創新」、「研究卓越」、「社會責任」為主軸，打造「教學、研究與社會責任」整體卓越的未來大學之願景邁進。</p>
                        <p>&nbsp &nbsp &nbsp &nbsp成大單車節，以成大學生最常使用之通勤工具而命名，傳達成大學生勤奮之正面形象，期許莘莘學子能與成大一同成長。</p>
                        <p>&nbsp &nbsp &nbsp &nbsp今年邁入第十四屆，在去年遭遇全球疫情停辦後，全國高中生、家長和業界專家都缺少了一了解、認識成功大學的最佳途徑。</p>
                        <br>
                        <p style="font-weight: bold; color: #30B6F5;"><b>&nbsp &nbsp &nbsp &nbsp「以成大為中心，探索過去，開拓未來。」</b></p>
                        <br>
                        <p>&nbsp &nbsp &nbsp &nbsp單車節立足成大，自南方躍起，放眼全台灣，引入北部豐富的教育資源，帶動南台灣創新教育與自我探索發展，而整體活動籌備依照以下三大宗旨：</p>
                        <br>
                        <p><span style="font-weight: bold; color: #30B6F5;">1. 探索科系：</span>認識成大十大學院、四十六個科系的學習環境、師資設備、就業出路等，讓全台高中生探索適合自己的學系，鍵結起高中端與大學端、弭平兩造之資訊落差。</p>
                        <p>另外，結合職涯發展，廣邀全台相關產業公司來到現場，與高中生，甚至是成大學生推廣互動。</p>
                        <br>
                        <p><span style="font-weight: bold; color: #30B6F5;">2. 探索成大：</span>適逢成大創校 90 周年，透過展場呈現成大過去歷史風華、未來創新趨勢，進而推廣成大教學環境特色與優勢。</p>
                        <p>近年，第十學院、敏求智慧運算學院、台灣半導體研究中心等創新基地，全台菁英亦皆有目共睹，必能吸引廣大學生家長前來一探究竟。</p>
                        <br>
                        <p><span style="font-weight: bold; color: #30B6F5;">3. 探索台南：</span>匯聚人潮，帶動周邊觀光，打造舒適、適合讀書的生活環境吸引高中生來台南就學，建立起南臺灣之首的正面形象。</p>
                        <p>結合在地歷史文化與特色小吃，呈現活力、熱情的成大人形象。</p>
                        <br>
                        <p>&nbsp &nbsp &nbsp &nbsp承載過去十三年來的籌備經驗、成功大學邁入九十周年的風華底蘊，由成功大學學生籌備，希望能共同承擔頂尖大學學生的社會責任，提供全台學子更新穎、多元的教育資訊！</p><br><br><br><br><br><br><br><br>`,
      pc: false
    }
  },
  updated: function () {
    var pc = this.pc
    if (this.titleFlag) {
      setTimeout(function () {
        var block = document.querySelectorAll('[data-block]')
        for (var i = 0; i < block.length; i++) {
          if (pc) {
            block[i].setAttribute('style', 'height: 28vh; transition: height .9s ease;')
          } else {
            block[i].setAttribute('style', 'height: 26vw; transition: height .9s ease;')
          }
        }
      }, 100)
      this.titleFlag = false
    }
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
    mapMaxMsg: function (event) {
      document.getElementsByClassName('map_msg')[0].setAttribute('style', 'display:absolute;' + 'left:' + (event.clientX - 100) + 'px; top:' + (event.clientY - 50) + 'px;' + 'background-color: rgb(254,246,231);' + 'box-shadow: 1px 1px 1px 2px rgb(102,102,102);')
      document.getElementsByClassName('map_msg')[0].innerHTML = '點按可放大'
    },
    mapMaxMsgOut: function (event) {
      document.getElementsByClassName('map_msg')[0].setAttribute('style', 'display:none;' + 'left:' + (event.clientX - 100) + 'px; top:' + (event.clientY - 50) + 'px;' + 'background-color: rgb(254,246,231);' + 'box-shadow: 1px 1px 1px 2px rgb(102,102,102);')
    },
    selectPageTopic: function (title, index) {
      var i, j
      this.currentIndex = title
      this.titleFlag = true
      var titleList = document.querySelectorAll('[data-key]')
      for (i = 0; i < titleList.length; i++) {
        if (i === title) {
          titleList[i].setAttribute('style', `
            background-color: rgb(44,185,244);`)
        } else {
          titleList[i].setAttribute('style', `
            background-color: rgb(117,210,243);`)
        }
      }
      if (title === 0) {
        this.layout1Index = index
        this.layout1PageSrc = srcJson.introduce[index]
        for (i = 0; i < this.layout1FlagArr.length; i++) {
          for (j = 0; j < this.layout1FlagArr[i].length; j++) {
            this.layout1FlagArr[i][j] = false
          }
        }
      }
      if (title === 1) {
        this.layout2Index = index
        this.layout2DaySrc = srcJson.time[index]
        var titles = document.querySelectorAll('[data-day]')
        for (i = 0; i < titles.length; i++) {
          if (i === index) {
            titles[i].setAttribute('style', `
              background-color: rgb(75,196,245);`)
          } else {
            titles[i].setAttribute('style', `
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
        if (this.pc) {
          block[index].setAttribute('style', 'height: 28vh; transition: height .9s ease;')
        } else {
          block[index].setAttribute('style', 'height: 26vw; transition: height .9s ease;')
        }
      } else {
        this.layout1FlagArr[this.layout1Index][index] = true
        block[index].setAttribute('style', 'height: auto; transition: height .9s ease;')
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
    }
  }
}
</script>

<style lang="scss" scoped>

  /*
    mobile layout css
  */
  @media only screen and (max-width: 599px) {
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
    @keyframes flow-in {
      from { right: -40%; }
      to { right: 0%; }
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
    .live_dot2 {
      z-index: 5;
      position: absolute;
      width: 60vw;
      height: 60vh;
      left: 0%;
      top: 20%;
      background-image: url('../assets//live/mobile/dot2.svg');
      background-repeat: no-repeat;
      background-size: contain;
      background-position: center center;
    }
    .live_dot3 {
      z-index: 5;
      position: absolute;
      width: 50vw;
      height: 50vh;
      right: 0%;
      top: 35%;
      background-image: url('../assets//live/mobile/dot3.svg');
      background-repeat: no-repeat;
      background-size: contain;
      background-position: center center;
    }
    .live_dot4 {
      z-index: 5;
      position: absolute;
      width: 60vw;
      height: 60vh;
      left: 0%;
      bottom: -15%;
      background-image: url('../assets//live/mobile/dot1.svg');
      background-repeat: no-repeat;
      background-size: contain;
      background-position: center center;
    }
    .activity_big_map {
      position: absolute;
      left: 0;
      top: 0;
      width: 100vw;
      height: 80vh;
      overflow-x: scroll;
      overflow-y: scroll;
      z-index: 100;
      display: flex;
      flex-direction: column;
      background-color: rgb(255, 255, 255);
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
      img {
        width: 100vh;
        height: 100vh;
      }
    }
    .activity_top_bar_mobile {
      position: absolute;
      display: grid;
      grid-template-columns: 20vw 1fr 20vw;
      grid-template-areas: "exit title list";
      justify-content: center;
      justify-items: center;
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
      .activity_mobile_exit_button {
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
      .activity_mobile_title {
        grid-area: title;
        width: 60vw;
        background-image: url('../assets//activity/title.svg');
        background-repeat: no-repeat;
        background-size: 75% 75%;
        background-position: center center;
      }
      .activity_mobile_list {
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
    .activity_mobile_list_area {
      position: absolute;
      display: grid;
      grid-template-rows: repeat(8, 7vh);
      z-index: 100;
      width: 40vw;
      height: 92vh;
      right: 0%;
      top: 8%;
      background-color: rgb(250, 242, 226);
      animation: flow-in 0.5s ease;
      label {
        color:rgb(75,196,245);
        line-height: 6vh;
        font-size: 2.3vh;
        font-weight: bold;
        letter-spacing: 2px;
        border: 1px solid rgb(200, 200, 200);
        &:hover {
          filter: brightness(150%);
        }
        &:active {
          filter: brightness(60%);
        }
      }
    }
    .activity_topic_list_mobile {
      position: absolute;
      top: 16vh;
      left: 5vw;
      width: 47vw;
      height: auto;
      max-height: 70vh;
      overflow-y: scroll;
      overflow-x: hidden;
      z-index: 50;

      display: flex;
      flex-direction: column;
      .title_block {
        display: flex;
        flex-direction: column;
        border: 1px solid rgb(255,240,218);
        .title_mobile {
          background-color: rgb(75,196,243);
          height: 5vh;
          color: white;
          height: 5vh;
          line-height: 5vh;
          font-size: 15px;
          font-weight: bold;
          letter-spacing: 0.2vw;
          text-align: center;
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
          height: auto;
          display: flex;
          flex-direction: column;
          label {
            width: 100%;
            height: 4.5vh;
            color: rgb(80,196,238);
            line-height: 4.5vh;
            font-size: 1.7vh;
            letter-spacing: 1px;
            text-align: center;
            &:hover {
              filter: brightness(120%);
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
      // z-index: 10;
    }
    .top_title {
      grid-area: topic;
      height: 8vh;
      width: 100vw;
      margin: 1vh 5vw;

      display: flex;
      justify-content: flex-start;
      align-items: center;
      .top_title_first {
        z-index: 100;
        width: 44vw;
        margin: 0 2vw 0 5vw;
        padding: 0.9vh 0 1.2vh 3vw;
        height: 2.8vh;
        border-radius: 10px;
        background-color: rgb(46,183,245);
        box-shadow: 1px 1px 2px 1px rgb(102,102,102);
        color: white;
        font-size: 15px;
        font-weight: bold;
        letter-spacing: 0.8vw;
        text-align: left;
        line-height: 3vh;
        display: grid;
        grid-template-columns: 1fr 4vh;
        grid-template-areas: ". arr";
        justify-content: center;
        align-items: center;
        &:hover {
          filter: rgb(75,196,243);
        }
        &:active {
          filter: brightness(60%);
          box-shadow: 0 0 1px 2px rgb(102,102,102);
        }
        .top_title_arrow {
          grid-area: arr;
          z-index: 20;
          width: 1.8vh;
          height: 1.8vh;
          margin: 0 0 0 5px;
          border: 5px solid transparent;
          border-radius: 10px;
          background-image: url('../assets//arrow.svg');
          background-repeat: no-repeat;
          background-size: contain;
          background-position: center bottom;
          transform: rotate(90deg);
        }
      }
      .top_topic {
        height: 5vh;
        width: auto;
        margin: 0;
        padding:0;
        label {
          border-radius: 0%;
          height: 7vh;
          margin: 0 2vw 0 3vw;
          padding: 0;
          background-color: transparent;
          color: rgb(75,196,245);
          font-size: 13px;
          line-height: 6vh;
          letter-spacing: 1px;
          text-align: left;
          background-image: url('../assets//activity/text_bottom.png');
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
      z-index: 10;
      .content1 {
        overflow-y: scroll;
        overflow-x: hidden;
        transition: filter .8s ease;
        display: flex;
        flex-direction: column;
        align-items: center;
        .block1 {
          width: 93%;
          height: 26vw;
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
            border-radius: 2vw;
            margin: 10% 10%;
            width: 32vw;
            height: 18vw;
            box-shadow: 0 0 1px 1px rgba(51, 51, 51, 0.5);
            transition: filter .5s ease;
            &:hover {
              filter: brightness(120%);
              transition: .5s ease;
              box-shadow: 1px 1px 2px 1px rgba(51, 51, 51, 0.5);
            }
            &:active {
              filter: brightness(60%);
            }
          }
          .title1 {
            grid-area: title;
            text-align: left;
            padding: 1.2vh 2vw 0 2vw;
            width: 90%;
            // height: 30px;
            color: white;
            // line-height: 26px;
            font-size: 1.8vh;
            font-weight: bold;
            letter-spacing: 0.2vw;
          }
          .time1 {
            grid-area: time;
            display: flex;
            padding: 0.2vh 2vw;
            label {
              border-radius: 3vw;
              background-color: rgb(255,241,191);
              padding: 0 1vw;
              width: auto;
              height: 2.6vh;
              line-height: 2.5vh;
              font-size: 1.5vh;
              text-align: left;
              color: rgb(153,145,114);
              font-weight: bold;
              letter-spacing: 0.1vw;
            }
          }
          .introduce1 {
            grid-area: introduce;
            padding: 0 2vw;
            margin: 0.5vh 0 1.5vh 0;
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
              line-height: 2.5vh;
              font-size: 1.7vh;
              color: white;
              letter-spacing: 0.1vh;
              text-align: left;
            }
          }
        }
        .block1_empty {
          width: 93%;
          height: 20vh;
        }
      }
    }
    .activity_layout2 {
      grid-area: content;
      overflow-y: scroll;
      overflow-x: hidden;
      height: 80vh;
      background-color: rgba(255, 255, 255, 0.6);
      z-index: 10;
      .content2 {
        width: 100%;
        height: 93%;
        padding: 1vh 1vw;
        overflow-y: scroll;
        .block2 {
          height: auto;
          width: 94vw;
          margin: 0 2.5vw 3vh 2.5vw;

          display: flex;
          flex-direction: column;
          .title2 {
            height: 5vh;
            width: 100%;
            text-align: left;
            padding: 0.5vh 2vw;
            color: rgb(75,196,245);
            font-size: 2vh;
            font-weight: bold;
            letter-spacing: 0.5vw;
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
                  height: 3vh;
                  padding: 1vh;
                  text-align: center;
                  vertical-align: middle;
                  border: 1px solid rgb(75,196,245);
                  color: white;
                  font-weight: bold;
                  font-size: 2vh;
                  letter-spacing: 0.5vw;
                  line-height: 3vh;
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
                  padding: 1vh 1.5vw;
                  text-align: center;
                  vertical-align: middle;
                  border: 1px solid rgb(75,196,245);
                  color: rgb(102,102,102);
                  line-height: 2.5vh;
                  font-size: 1.7vh;
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
          table[data-table="table_1_1"] {
            td[data-td="td_1"] {
              text-align: left;
              width: 40%;
            }
          }
          table[data-table="table_1_2"] {
            td[data-td="td_1"] {
              text-align: left;
              width: 30%;
            }
            td[data-td="td_2"] {
              text-align: center;
              width: 8%;
            }
            td[data-td="td_3"] {
              text-align: left;
              width: 32%;
            }
          }
          table[data-table="table_1_3"] {
            td[data-td="td_1"] {
              text-align: left;
              width: 30%;
            }
            td[data-td="td_2"] {
              text-align: center;
              width: 8%;
            }
            td[data-td="td_3"] {
              text-align: left;
              width: 37%;
            }
          }
          table[data-table="table_1_4"] {
            td[data-td="td_1"] {
              text-align: center;
              width: 22%;
            }
            td[data-td="td_2"] {
              text-align: left;
              width: 40%;
            }
            td[data-td="td_3"] {
              text-align: center;
              width: 18%;
            }
          }
          table[data-table="table_1_5"] {
            td[data-td="td_1"] {
              text-align: center;
              width: 25%;
            }
            td[data-td="td_2"] {
              padding: 0 0 0 3vw;
              text-align: left;
              width: 40%;
            }
            td[data-td="td_3"] {
              text-align: center;
              width: 18%;
            }
          }
          table[data-table="table_2_1"] {
            td[data-td="td_1"] {
              text-align: left;
              width: 24%;
            }
            td[data-td="td_2"] {
              text-align: center;
              width: 25%;
            }
            td[data-td="td_3"] {
              text-align: center;
              width: 18%;
            }
          }
          table[data-table="table_2_2"] {
            td[data-td="td_1"] {
              text-align: left;
              width: 30%;
            }
            td[data-td="td_2"] {
              text-align: center;
              width: 8%;
            }
            td[data-td="td_3"] {
              text-align: left;
              width: 25%;
            }
          }
          table[data-table="table_2_3"] {
            td[data-td="td_1"] {
              text-align: left;
              width: 37%;
            }
            td[data-td="td_2"] {
              text-align: center;
              width: 4%;
            }
            td[data-td="td_3"] {
              text-align: left;
              width: 34%;
            }
          }
          table[data-table="table_2_4"] {
            td[data-td="td_1"] {
              text-align: center;
              width: 19%;
            }
            td[data-td="td_2"] {
              text-align: left;
              width: 34%;
            }
            td[data-td="td_3"] {
              text-align: center;
              width: 27%;
            }
          }
          table[data-table="table_2_5"] {
            td[data-td="td_1"] {
              text-align: center;
              width: 30%;
            }
            td[data-td="td_2"] {
              text-align: left;
              width: 50%;
            }
          }
        }
        .content2_empty {
          height: 20vh;
          width: 100%;
        }
      }
    }
    .activity_layout3 {
      grid-area: content;
      overflow-y: scroll;
      overflow-x: hidden;
      height: 80vh;
      z-index: 10;

      display: flex;
      flex-direction: column;
      align-items: flex-start;
      justify-content: flex-start;
      .map_block {
        width: 94vw;
        height: 94vw;
        .map_img {
          width: 90vw;
          height: 90vw;
          border-radius: 30px;
          box-shadow: 1px 1px 0 1px rgb(102,102,102);
          &:active {
            width: 91vw;
            height: 91vw;
            filter: brightness(60%);
          }
        }
      }
      .map_big_msg {
        width: auto;
        height: 26px;
        margin: 0 0 0 5vw;
        padding: 0 3vw;
        text-align: left;
        line-height: 25px;
        background-color: rgb(103, 192, 225);
        border-radius: 10px;
        color: white;
        box-shadow: 1px 1px 0 1px rgb(102,102,102);
      }
    }
    .activity_layout4 {
      grid-area: content;
      height: 80vh;
      z-index: 10;
      .block4 {
        width: 96vw;
        height: 67vh;
        overflow-y: scroll;
        overflow-x: hidden;
        img {
          width: 96vw;
          height: 54vw;
          border-radius: 20px;
        }
        article {
          padding: 20px 10px;
          text-align: left;
          color: rgb(102,102,102);
          background-color: rgba(255, 255, 255, 0.6);
          font-size: 15px;
          font-weight: 400;
          line-height: 20px;
          letter-spacing: 1px;
        }
      }
    }
  }
  /*
    computer layout css
  */
  @media only screen and (min-width: 600px) {
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
      height: 100%;
      width: 100%;
      min-width: 600px;
      margin: 0;
      padding: 0;
      background: white;
      overflow: hidden;
      .activity_big_map {
        width: 100vw;
        height: 100vh;
        background-size: contain;
        background-repeat: no-repeat;
        background-position: center center;
        transition: background-color 2.5s ease;
        background-color: rgba(255, 255, 255, 0.8);
        z-index: 1000;
        overflow: scroll;
        img {
          width: 95vw;
          height: 95vw;
        }
      }
    }
    .activity_top_bar_pc {
      position: absolute;
      display: flex;
      justify-content: center;
      min-width: 600px;
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
          background-image: url("../assets//logoHome.svg");
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
            font-size: calc(6px + 1vw);
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
      background-image: url('../assets//flower.svg');
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
      min-width: 350px;
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
      label[data-key="title_1"] {
        background-color: rgb(44,185,244);
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
      min-width: 550px;
      grid-area: page;
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      // overflow-y: scroll;
      overflow: hidden;
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

        display: flex;
        flex-direction: column;
        align-items: flex-start;
        .block1 {
          width: 52vw;
          height: 28vh;
          background-color: rgb(45,184,245);
          border-radius: 2vw;
          margin: 1vw 1.5vw 1vw 0;
          box-shadow: 0 0 3px 1px rgba(51, 51, 51, 0.5);

          display: grid;
          grid-template-columns: 3fr 4fr;
          grid-template-rows: 7vh 4vh 1fr;
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
            border-radius: 0.6vw;
            margin: 4vh 2vw;
            width: 35.2vh;
            height: 20vh;
            min-width: 160px;
            min-height: 90px;
            box-shadow: 0 0 3px 2px rgba(51, 51, 51, 0.5);
          }
          .title1 {
            grid-area: title;
            text-align: left;
            padding: 3vh 0 0 0.2vw;
            width: 90%;
            height: 3vh;
            color: white;
            line-height: 3vh;
            font-size: 2.5vh;
            font-weight: bold;
            letter-spacing: 0.2vw;
          }
          .time1 {
            grid-area: time;
            display: flex;
            label {
              min-height: 16px;
              border-radius: 1vw;
              background-color: rgb(255,241,191);
              padding: 0 10px;
              width: auto;
              height: 3vh;
              line-height: 3vh;
              font-size: 2vh;
              text-align: left;
              color: rgb(153,145,114);
              font-weight: bold;
              letter-spacing: 3px;
            }
          }
          .introduce1 {
            grid-area: introduce;
            margin: 0 0 3vh 0;
            width: 97%;
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
              font-size: 2.2vh;
              color: white;
              letter-spacing: 2px;
              text-align: left;
            }
          }
        }
      }
    }
    .activity_layout2 {
      min-width: 550px;
      grid-area: page;
      left: 0%;
      top: 0%;
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      overflow: hidden;
      z-index: 5;
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
        width: 96%;
        height: 93%;
        padding: 1vh 1vw;
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
        .img4 {
          margin: 0 0 20px 0;
          width: 48vw;
          height: 27vw;
          border-radius: 1vw;
        }
        .block2 {
          width: 96%;
          height: auto;
          display: grid;
          grid-template-rows: 9vh 1fr;
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
                  height: 4vh;
                  padding: 3px;
                  text-align: center;
                  vertical-align: middle;
                  border: 1px solid rgb(75,196,245);
                  color: white;
                  font-weight: bold;
                  font-size: 2.4vh;
                  letter-spacing: 2px;
                  line-height: 2.5vh;
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
                  height: 3vh;
                  padding: 5px 10px;
                  text-align: center;
                  vertical-align: middle;
                  border: 1px solid rgb(75,196,245);
                  color: rgb(102,102,102);
                  line-height: 3vh;
                  font-size: 2vh;
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
          table[data-table="table_1_1"] {
            td[data-td="td_1"] {
                text-align: left;
                width: 45%;
            }
          }
          table[data-table="table_1_2"] {
            td[data-td="td_1"] {
                text-align: left;
                width: 35%;
            }
            td[data-td="td_2"] {
                text-align: center;
                width: 12%;
            }
            td[data-td="td_3"] {
                text-align: left;
                width: 30%;
            }
          }
          table[data-table="table_1_3"] {
            td[data-td="td_1"] {
                text-align: left;
                width: 27%;
            }
            td[data-td="td_2"] {
                text-align: center;
                width: 12%;
            }
            td[data-td="td_3"] {
                text-align: left;
                width: 42%;
            }
          }
          table[data-table="table_1_4"] {
            td[data-td="td_1"] {
                text-align: center;
                width: 25%;
            }
            td[data-td="td_2"] {
                text-align: left;
                width: 40%;
            }
            td[data-td="td_3"] {
                text-align: center;
                width: 18%;
            }
          }
          table[data-table="table_2_2"] {
            td[data-td="td_1"] {
                text-align: left;
                width: 32%;
            }
            td[data-td="td_2"] {
                text-align: center;
                width: 12%;
            }
            td[data-td="td_3"] {
                text-align: left;
                width: 30%;
            }
          }
          table[data-table="table_2_3"] {
            td[data-td="td_1"] {
                text-align: left;
                width: 32%;
            }
            td[data-td="td_2"] {
                text-align: center;
                width: 12%;
            }
            td[data-td="td_3"] {
                text-align: left;
                width: 32%;
            }
          }
        }
      }
    }
    .activity_layout3 {
      min-width: 550px;
      position: absolute;
      left: 5vw;
      top: 0;
      width: 60vw;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      overflow: hidden;
      z-index: 5;
      .map_block {
        width: 75vh;
        height: 75vh;
        padding: 3vh 2vw;
        border-radius: 2vh;
        background-color: rgb(62,186,233);
        display: flex;
        justify-content: center;
        align-items: center;
        .map_img {
          border-radius: 2vh;
          width: 100%;
          height: 100%;
          &:hover {
            filter: brightness(120%);
            transition: filter .3s ease;
            width: 102%;
            height: 102%;
          }
          &:active {
            filter: brightness(60%);
          }
        }
        .map_msg {
          position: absolute;
          width: 120px;
          height: 25px;
          border-radius: 10px;
          line-height: 25px;
          color: rgb(62,186,233);
          font-size: 15px;
          font-weight: bold;
          letter-spacing: 0.2vw;
        }
      }
    }
    .activity_layout4 {
      min-width: 550px;
      position: absolute;
      left: 2%;
      top: 2%;
      width: 50vw;
      height: 80vh;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      // overflow-y: scroll;
      z-index: 5;
      .block4 {
        width: 100%;
        height: 100%;
        overflow-y: scroll;
        overflow-x: hidden;
        padding: 0 2vw 0 0;
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
        .img4 {
          margin: 0 0 20px 0;
          width: 48vw;
          height: 27vw;
          border-radius: 1vw;
        }
        article {
          width: 48vw;
          text-align: left;
          color: rgb(102,102,102);
          font-size: 15px;
          font-weight: 400;
          line-height: 20px;
          letter-spacing: 1px;
        }
      }
    }
  }
</style>
