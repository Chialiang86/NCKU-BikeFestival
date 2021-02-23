<template lang="pug">
  div(class="dept_page")
    div(class="dept_top_bar_pc")
      div(class="dept_top_bar_layout" @click="scroll()")
        router-link(tag="label" class="dept_exit_button" to="/")
        div(class="dept_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===2")
          label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="pc") 我要報名
    div(class="dept_top_bar_mobile")
      div(class="dept_mobile_title" @click="list = false")
      router-link(tag="div" class="dept_mobile_exit_button" to="/")
      div(class="dept_mobile_list" @click="list = !list")
    div(class="dept_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="!pc") 我要報名
    div(class="dept_decoration_top")
    div(class="dept_decoration_bottom")
      //- g
        ellipse(cx="90%" cy="540" rx="50%" ry="100" stroke="#dddddd" stroke-width="5px" fill="none")
    div(class="dept_layout" @click="list = false")
      section(class="dept_list_section")
        div(class="dept_class")
          ul
            li(v-for="(iter, index) in classes" v-bind:key="`${index}-${iter}`" v-bind:data-key="classKeys[index]" class="dept_class_item" v-on:click="chooseClass(index)" v-bind:class="{active: currentIndex === index}" )
              p {{iter}}
      section(class="dept_chain_section")
        div(id="small_chain" class="dept_chain_small")
        div(id="large_chain" class="dept_chain_large")
        div(id="large_chain_mobile" class="dept_chain_large_mobile")
        svg(width="56%" height="56%" viewBox="-10 -70 950 500")
          g(id="chain_path")
            path(d="M854.4800244140624,253.27999999999997 a66.84,66.84 0 0 0 0,-132.24 L218.7800244140625,2.7799999999999727 A187.68,187.68 0 0 0 186.50002441406252,0 c-103,0 -186.5,83.5 -186.5,186.5 s83.5,186.5 186.5,186.5 a187.68,187.68 0 0 0 32.25,-2.7800000000000002 z" fill="none" stroke-linejoin="round"  stroke-linecap="round" stroke-width="8px" stroke="#2DB8F5" stroke-dasharray="1, 20" stroke-miterlimit="10")
        div(v-for="(iter, index) in classKeys" v-bind:key="`${index}-${iter}-college`" class="dept_college" v-bind:class="deptBinding(index)" v-bind:style="showDeptCollege ? {} : { opacity: 1 }")
          p(v-for="(name, index) in colleges[iter]" v-bind:key="`${index}-${name}-college`") {{name}}
</template>

<script>
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
      classes: ['規劃與設計學院', '社會科科學院', '不分學院', '工學院', '理學院', '文學院', '醫學院', '管理學院', '電機資訊學院', '生物科學與科技學院'],
      classKeys: ['design', 'social', 'undeclear', 'engineer', 'science', 'humanity', 'medicine', 'management', 'computer', 'biological'],
      colleges: {
        design: ['建築', '都計', '工設'],
        social: ['政治', '經濟', '法律', '心理'],
        undeclear: ['不分系學程'],
        engineer: ['航太', '機械', '化工', '土木', '材料', '水利', '工科', '系統'],
        science: ['數學', '化學', '物理', '地科', '光電'],
        humanity: ['中文', '外文', '歷史', '臺文'],
        medicine: ['醫學', '藥學', '護理', '物治', '職治', '醫技'],
        management: ['會計', '統計', '企管', '交管', '工資管'],
        computer: ['資訊', '電機'],
        biological: ['生科', '生技']
      },
      currentIndex: -1,
      check: false
    }
  },
  computed: {
    showDeptCollege: function () {
      return this.check
    }
  },
  mounted: function () {
    const self = this
    this.pc = this.isPC()
    this.setBarHeight()
    setTimeout(function () {
      self.check = true
    }, 500)
  },
  methods: {
    windowSizeChange: function (event) {
      if (window.innerWidth > 551) {
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
    chooseClass: function (index) {
      if (this.currentIndex === index) {
        return
      }
      const chainPath = this.$el.querySelector('#chain_path path')
      const smallChain = this.$el.querySelector('#small_chain')
      const largeChain = this.$el.querySelector('#large_chain')
      const animEndEvent = function () {
        chainPath.classList.remove('chain-path-anim')
        smallChain.classList.remove('chain-rotate-anim')
        largeChain.classList.remove('chain-rotate-anim')
        chainPath.removeEventListener('animationend', animEndEvent)
        smallChain.removeEventListener('animationend', animEndEvent)
        largeChain.removeEventListener('animationend', animEndEvent)
      }
      // remove animation class
      animEndEvent()
      this.currentIndex = index
      // animate the chain path
      chainPath.classList.add('chain-path-anim')
      // animate the chain
      smallChain.classList.add('chain-rotate-anim')
      largeChain.classList.add('chain-rotate-anim')
      // config animation end event to remove animation class
      chainPath.addEventListener('animationend', animEndEvent)
      smallChain.addEventListener('animationend', animEndEvent)
      largeChain.addEventListener('animationend', animEndEvent)
    },
    deptBinding: function (index) {
      const displayName = `rotate-${this.classKeys[index]}`
      const disappearName = `rotate-${this.classKeys[index]}-disappear`
      const result = {}
      result[displayName] = this.currentIndex === index
      result[disappearName] = this.currentIndex !== index
      return result
    }
  }
}
</script>

<style lang="scss" scoped>

@keyframes path {
  to { stroke-dashoffset: 100; }
}

@keyframes chain {
  0% { transform: rotateZ(0deg); }
  100% { transform: rotateZ(60deg); }
}

.chain-path-anim {
  stroke-dashoffset: 0;
  animation: path .5s ease forwards;
}

.chain-rotate-anim {
  animation: chain .5s ease forwards;
}

.hidden-dept-college {
  opacity: 0 !important;
}

/* 2 components rotate */
@keyframes rotate-45 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-45deg) translateX(36vw) rotate(45deg); }
}
@keyframes rotate-135 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(45deg) translateX(36vw) rotate(-45deg); }
}
@keyframes rotate-45-disappear {
  0% { opacity: 1; transform: rotate(-45deg) translateX(36vw) rotate(45deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-135-disappear {
  0% { opacity: 1; transform: rotate(45deg) translateX(36vw) rotate(-45deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}

/* 3 components rotate*/
@keyframes rotate-30 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-60deg) translateX(36vw) rotate(60deg); }
}
@keyframes rotate-90 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(0deg) translateX(36vw) rotate(0deg); }
}
@keyframes rotate-150 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(60deg) translateX(36vw) rotate(-60deg); }
}

@keyframes rotate-30-disappear {
  0% { opacity: 1; transform: rotate(-60deg) translateX(36vw) rotate(60deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-90-disappear {
  0% { opacity: 1; transform: rotate(0deg) translateX(36vw) rotate(0deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-150-disappear {
  0% { opacity: 1; transform: rotate(60deg) translateX(36vw) rotate(-60deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}

/* 4 components roate*/
@keyframes rotate-23 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-67deg) translateX(36vw) rotate(67deg); }
}
@keyframes rotate-68 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-23deg) translateX(36vw) rotate(23deg); }
}
@keyframes rotate-113 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(23deg) translateX(36vw) rotate(-23deg); }
}
@keyframes rotate-158 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(68deg) translateX(36vw) rotate(-68deg); }
}

@keyframes rotate-23-disappear {
  0% { opacity: 1; transform: rotate(-67deg) translateX(36vw) rotate(67deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-68-disappear {
  0% { opacity: 1; transform: rotate(-23deg) translateX(36vw) rotate(23deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-113-disappear {
  0% { opacity: 1; transform: rotate(23deg) translateX(36vw) rotate(-23deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-158-disappear {
  0% { opacity: 1; transform: rotate(68deg) translateX(36vw) rotate(-68deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}

/* 5 components rotate */
@keyframes rotate-18 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-72deg) translateX(36vw) rotate(72deg); }
}
@keyframes rotate-54 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-36deg) translateX(36vw) rotate(36deg); }
}
/*
@keyframes rotate-90 {
  0% { transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { transform: rotate(0deg) translateX(36vw) rotate(0deg); }
}
*/
@keyframes rotate-126 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(36deg) translateX(36vw) rotate(-36deg); }
}
@keyframes rotate-162 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(72deg) translateX(36vw) rotate(-72deg); }
}

@keyframes rotate-18-disappear {
  0% { opacity: 1; transform: rotate(-72deg) translateX(36vw) rotate(72deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-54-disappear {
  0% { opacity: 1; transform: rotate(-36deg) translateX(36vw) rotate(36deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}

@keyframes rotate-126-disappear {
  0% { opacity: 1; transform: rotate(36deg) translateX(36vw) rotate(-36deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-162-disappear {
  0% { opacity: 1; transform: rotate(72deg) translateX(36vw) rotate(-72deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}

/* 6 components rotate */

@keyframes rotate-15 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-75deg) translateX(36vw) rotate(75deg); }
}
/*
@keyframes rotate-45 {
  0% { transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { transform: rotate(-45deg) translateX(36vw) rotate(45deg); }
}
*/
@keyframes rotate-75 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-15deg) translateX(36vw) rotate(15deg); }
}
@keyframes rotate-105 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(15deg) translateX(36vw) rotate(-15deg); }
}
/*
@keyframes rotate-135 {
  0% { transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { transform: rotate(45deg) translateX(36vw) rotate(-45deg); }
}
*/
@keyframes rotate-165 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(75deg) translateX(36vw) rotate(-75deg); }
}

@keyframes rotate-15-disappear {
  0% { opacity: 1; transform: rotate(-75deg) translateX(36vw) rotate(75deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}

@keyframes rotate-75-disappear {
  0% { opacity: 1; transform: rotate(-15deg) translateX(36vw) rotate(15deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-105-disappear {
  0% { opacity: 1; transform: rotate(15deg) translateX(36vw) rotate(-15deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}

@keyframes rotate-165-disappear {
  0% { opacity: 1; transform: rotate(75deg) translateX(36vw) rotate(-75deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}

/* 7 components rotate */

@keyframes rotate-12 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-78deg) translateX(36vw) rotate(78deg); }
}
@keyframes rotate-37 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-53deg) translateX(36vw) rotate(53deg); }
}
@keyframes rotate-63 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-27deg) translateX(36vw) rotate(27deg); }
}
/*
@keyframes rotate-90 {
  0% { transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { transform: rotate(0deg) translateX(36vw) rotate(-0deg); }
}
*/
@keyframes rotate-117 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(27deg) translateX(36vw) rotate(-27deg); }
}
@keyframes rotate-143 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(53deg) translateX(36vw) rotate(-53deg); }
}
@keyframes rotate-168 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(78deg) translateX(36vw) rotate(-78deg); }
}

@keyframes rotate-12-disappear {
  0% { opacity: 1; transform: rotate(-78deg) translateX(36vw) rotate(78deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-37-disappear {
  0% { opacity: 1; transform: rotate(-53deg) translateX(36vw) rotate(53deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-63-disappear {
  0% { opacity: 1; transform: rotate(-27deg) translateX(36vw) rotate(27deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-117-disappear {
  0% { opacity: 1; transform: rotate(27deg) translateX(36vw) rotate(-27deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-143-disappear {
  0% { opacity: 1; transform: rotate(53deg) translateX(36vw) rotate(-53deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-168-disappear {
  0% { opacity: 1; transform: rotate(78deg) translateX(36vw) rotate(-78deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}

/* 8 components rotate */

@keyframes rotate-11 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(38vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-88deg) translateX(38vw) rotate(88deg); }
}
@keyframes rotate-33 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-57deg) translateX(36vw) rotate(57deg); }
}
@keyframes rotate-56 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-34deg) translateX(36vw) rotate(34deg); }
}
@keyframes rotate-79 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-11deg) translateX(36vw) rotate(11deg); }
}
/*
@keyframes rotate-90 {
  0% { transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { transform: rotate(0deg) translateX(36vw) rotate(-0deg); }
}
*/
@keyframes rotate-101 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(11deg) translateX(36vw) rotate(-11deg); }
}
@keyframes rotate-124 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(34deg) translateX(36vw) rotate(-34deg); }
}
@keyframes rotate-147 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(36vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(57deg) translateX(36vw) rotate(-57deg); }
}
@keyframes rotate-169 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(38vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(88deg) translateX(38vw) rotate(-88deg); }
}

@keyframes rotate-11-disappear {
  0% { opacity: 1; transform: rotate(-88deg) translateX(38vw) rotate(88deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(38vw) rotate(-90deg); }
}
@keyframes rotate-33-disappear {
  0% { opacity: 1; transform: rotate(-57deg) translateX(36vw) rotate(57deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-56-disappear {
  0% { opacity: 1; transform: rotate(-34deg) translateX(36vw) rotate(34deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-79-disappear {
  0% { opacity: 1; transform: rotate(-11deg) translateX(36vw) rotate(11deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-101-disappear {
  0% { opacity: 1; transform: rotate(11deg) translateX(36vw) rotate(-11deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-124-disappear {
  0% { opacity: 1; transform: rotate(34deg) translateX(36vw) rotate(-34deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-147-disappear {
  0% { opacity: 1; transform: rotate(57deg) translateX(36vw) rotate(-57deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(36vw) rotate(-90deg); }
}
@keyframes rotate-169-disappear {
  0% { opacity: 1; transform: rotate(88deg) translateX(38vw) rotate(-88deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(38vw) rotate(-90deg); }
}

  /*
    phone layout css
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
    @keyframes flow-in {
      from { right: -40%; }
      to { right: 0%; }
    }
    .dept_page {
      display: flex;
      align-items: center;
      justify-content: flex-start;
      justify-items: flex-start;
      position: relative;
      height: 100vh;
      width: 100vw;
      background: rgb(255, 246, 232);
      margin: 0;
      padding: 0;
    }
    .dept_top_bar_mobile {
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
      .dept_mobile_exit_button {
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
      .dept_mobile_title {
        grid-area: title;
        width: 60vw;
        background-image: url('../assets/14/dept/title.svg');
        background-repeat: no-repeat;
        background-size: 75% 75%;
        background-position: center center;
      }
      .dept_mobile_list {
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
    .dept_mobile_list_area {
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

    .dept_decoration_top {
      display: none;
    }

    .dept_decoration_bottom {
      display: none;
    }

    .external_circle {
      display: none;
    }

    .dept_layout {
      width: 100vw;
      height: 100vh;
    }

    .dept_chain_section {

      width: 100%;
      height: 100%;

      .dept_chain_small {
        display: none;
      }

      .dept_chain_large {
        z-index:1;
        position: absolute;
        left: -58vh;
        top: -10vh;

        width: 120vh;
        height: 120vh;

        background-image: url("../assets/dept/large_chain.svg");
        opacity:0.8;
        background-repeat: no-repeat;
        background-size: 100% 100%;
        background-position: 50% 50%;

        pointer-events: none;
      }
      svg {
        position: absolute;
        display: none;
        top: 0;
        bottom: 0;
        margin: auto 0;
        left: -145vw;

        transform: scale(6);
        pointer-events: none;
      }
    }

    .dept_college {
      position: absolute;
      z-index: 10;
      left: -20vw;
      top: 47vh;
      box-sizing: border-box;

      will-change: opacity, transform;
      // transform: scale(2);
      p {
        z-index: 10;
        position: absolute;
        left: 0;
        top: 0;
        max-width: 30vw;
        width: 20vw;
        height: 2.6vh;
        text-align: center;
        vertical-align: bottom;
        background-color: rgb(79, 224, 224);
        margin: 2vh 0 0 20vw;
        color: white;
        border-radius: 5px;
        padding: 2px;
        font-size: 1.8vh;
        line-height: 2.4vh;
      }
      p:hover {
        z-index: 999;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.3);
      }
      &.rotate-design > p:nth-child(1) {
        animation: rotate-30 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-design > p:nth-child(2) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-design > p:nth-child(3) {
        animation: rotate-150 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-design-disappear > p:nth-child(1) {
        animation: rotate-30-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-design-disappear > p:nth-child(2) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-design-disappear > p:nth-child(3) {
        animation: rotate-150-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-social > p:nth-child(1) {
        animation: rotate-23 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social > p:nth-child(2) {
        animation: rotate-68 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social > p:nth-child(3) {
        animation: rotate-113 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social > p:nth-child(4) {
        animation: rotate-158 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-social-disappear > p:nth-child(1) {
        animation: rotate-23-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social-disappear > p:nth-child(2) {
        animation: rotate-68-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social-disappear > p:nth-child(3) {
        animation: rotate-113-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social-disappear > p:nth-child(4) {
        animation: rotate-158-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-undeclear > p {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-undeclear-disappear > p {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-engineer > p:nth-child(1) {
        animation: rotate-11 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(2) {
        animation: rotate-33 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(3) {
        animation: rotate-56 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(4) {
        animation: rotate-79 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(5) {
        animation: rotate-101 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(6) {
        animation: rotate-124 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(7) {
        animation: rotate-147 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(8) {
        animation: rotate-169 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      // &.rotate-engineer-2 > p:nth-child(1) {
      //   animation: rotate-15 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(2) {
      //   animation: rotate-45 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(3) {
      //   animation: rotate-75 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(4) {
      //   animation: rotate-105 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(5) {
      //   animation: rotate-135 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(6) {
      //   animation: rotate-165 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }

      &.rotate-engineer-disappear > p:nth-child(1) {
        animation: rotate-11-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(2) {
        animation: rotate-33-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(3) {
        animation: rotate-56-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(4) {
        animation: rotate-79-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(5) {
        animation: rotate-101-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(6) {
        animation: rotate-124-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(7) {
        animation: rotate-147-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(8) {
        animation: rotate-169-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      // &.rotate-engineer-2-disappear > p:nth-child(1) {
      //   animation: rotate-1-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(2) {
      //   animation: rotate-45-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(3) {
      //   animation: rotate-75-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(4) {
      //   animation: rotate-105-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(5) {
      //   animation: rotate-135-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(6) {
      //   animation: rotate-165-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }

      &.rotate-science > p:nth-child(1) {
        animation: rotate-18 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science > p:nth-child(2) {
        animation: rotate-54 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science > p:nth-child(3) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science > p:nth-child(4) {
        animation: rotate-126 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science > p:nth-child(5) {
        animation: rotate-162 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-science-disappear > p:nth-child(1) {
        animation: rotate-18-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science-disappear > p:nth-child(2) {
        animation: rotate-54-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science-disappear > p:nth-child(3) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science-disappear > p:nth-child(4) {
        animation: rotate-126-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science-disappear > p:nth-child(5) {
        animation: rotate-162-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-humanity > p:nth-child(1) {
        animation: rotate-23 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity > p:nth-child(2) {
        animation: rotate-68 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity > p:nth-child(3) {
        animation: rotate-113 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity > p:nth-child(4) {
        animation: rotate-158 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-humanity-disappear > p:nth-child(1) {
        animation: rotate-23-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity-disappear > p:nth-child(2) {
        animation: rotate-68-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity-disappear > p:nth-child(3) {
        animation: rotate-113-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity-disappear > p:nth-child(4) {
        animation: rotate-158-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-medicine > p:nth-child(1) {
        animation: rotate-15 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(2) {
        animation: rotate-45 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(3) {
        animation: rotate-75 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(4) {
        animation: rotate-105 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(5) {
        animation: rotate-135 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(6) {
        animation: rotate-165 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-medicine-disappear > p:nth-child(1) {
        animation: rotate-15-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(2) {
        animation: rotate-45-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(3) {
        animation: rotate-75-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(4) {
        animation: rotate-105-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(5) {
        animation: rotate-135-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(6) {
        animation: rotate-165-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-management > p:nth-child(1) {
        animation: rotate-18 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management > p:nth-child(2) {
        animation: rotate-54 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management > p:nth-child(3) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management > p:nth-child(4) {
        animation: rotate-126 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management > p:nth-child(5) {
        animation: rotate-162 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-management-disappear > p:nth-child(1) {
        animation: rotate-18-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management-disappear > p:nth-child(2) {
        animation: rotate-54-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management-disappear > p:nth-child(3) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management-disappear > p:nth-child(4) {
        animation: rotate-126-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management-disappear > p:nth-child(5) {
        animation: rotate-162-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-computer > p:nth-child(1) {
        animation: rotate-45 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-computer > p:nth-child(2) {
        animation: rotate-135 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-computer-disappear > p:nth-child(1) {
        animation: rotate-45-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-computer-disappear > p:nth-child(2) {
        animation: rotate-135-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-biological > p:nth-child(1) {
        animation: rotate-45 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-biological > p:nth-child(2) {
        animation: rotate-135 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-biological-disappear > p:nth-child(1) {
        animation: rotate-45-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-biological-disappear > p:nth-child(2) {
        animation: rotate-135-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

    }

    .dept_list_section {
      z-index:10;
      position: absolute;
      left: 16vw;
      top: 20vh;

      max-width: 30vh;

      ul {
        list-style-type: none;
        margin: 0;
        padding: 0;

        li {
          margin: 0;
          padding: 0;

          transition-property: font-size, text-align, background-color, padding;
          transition-duration: .3s;
          transition-timing-function: ease;
        }

        li[data-key="design"]  {
          transform: translateX(20px);
        }
        li[data-key="social"]  {
          transform: translateX(52px);
        }
        li[data-key="undeclear"]  {
          transform: translateX(76px);
        }
        li[data-key="engineer"]  {
          transform: translateX(93px);
        }
        li[data-key="engineer-2"]  {
          transform: translateX(100px);
        }
        li[data-key="science"]  {
          transform: translateX(100px);
        }
        li[data-key="humanity"]  {
          transform: translateX(100px);
        }
        li[data-key="medicine"]  {
          transform: translateX(98px);
        }
        li[data-key="management"]  {
          transform: translateX(88px);
        }
        li[data-key="computer"]  {
          transform: translateX(72px);
        }
        li[data-key="biological"]  {
          transform: translateX(46px);
        }
      }

      .dept_class_item {
        margin: 3vh;
        z-index:10;
        font-size: 15px;
        width: 45vw;

        p {
          width: 100%;

          margin: 0;
          padding: 10;

          text-align: right;
          border-radius: 10px;
          color: rgb(120, 120, 120);
          font-weight: 500;
          cursor: pointer;
        }
        &.active {
            color: white;
            border-radius: 10px;
            // background-color: rgb(103,193,225);
            padding: 6px;
            font-size: 17px;
          font-weight: bold;
            }
        &:hover {
          p {
            color: rgb(103, 192, 225);
            padding: 6px;
            font-size: 17px;
            font-weight: bold;
            border-radius: 10px;
            letter-spacing: 1px;
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

    .dept_page {
      position: absolute;
      display: flex;
      align-items: center;
      justify-content: flex-start;
      justify-items: flex-start;
      height: 100vh;
      width: 100vw;
      min-width: 1000px;
      background: rgb(255, 246, 232);
      margin: 0;
      padding: 0;
      overflow: hidden;
    }
    .dept_top_bar_pc {
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
      .dept_top_bar_layout {
        display: grid;
        grid-template-columns: 12vw 82vw 6vw;
        grid-template-areas: "home items .";
        justify-content: center;
        justify-items: center;
        align-items: center;
        align-content: center;
        .dept_exit_button {
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
        .dept_top_bar_item {
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
        .dept_sign_up_button {
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

    .dept_decoration_top {
      position: absolute;
      left: 46vw;
      top: 6vh;
      width:  30vw;

      margin-top: auto;
      margin-bottom: auto;
      padding-bottom: 38vw;

      // background: url("../assets/dept/decoration_top.svg");
      background-position: center top;
      background-repeat: no-repeat;
      background-size: contain;
    }

    .dept_decoration_bottom {
      position: absolute;
      top: -20vh;
      left: -2vw;
      height: 30vw;
      width: 25vw;

      margin-top: auto;
      margin-bottom: auto;
      padding-bottom: 38vw;

      background: url("../assets/dept/decoration_bottom.svg");
      background-position: center top;
      background-repeat: no-repeat;
      background-size: contain;
    }

    .external_circle {
      position: absolute;
      pointer-events: none;
    }

    .dept_layout {
      display: grid;
      grid-template-columns: 22vw 78vw;
      grid-template-areas: "list chain";

      justify-content: center;
      justify-items: center;
      align-items: center;

      width: 100vw;
      height: 100vh;
    }

    .dept_chain_section {
      grid-area: chain;

      display: grid;
      grid-template-columns: 4vw repeat(5, 1fr);
      grid-template-rows: 5vw repeat(3, 1fr) 5vw;
      grid-template-areas: ". . . . . exit"
        ". . large large large large"
        ". small large large large large "
        ". . large large large large"
        ". . . . . .";
      justify-content: center;
      justify-items: center;
      align-items: center;

      width: 100%;
      height: 100%;

      .dept_chain_small {
        grid-area: small;

        left: 50vw;
        top: 20vh;
        width: 13vw;
        height: 13vw;

        background-image: url("../assets/dept/small_chain.svg");
        background-repeat: no-repeat;
        background-size: 100% 100%;
        background-position: 50% 50%;
      }

      .dept_chain_large {
        grid-area: large;

        width: 35vw;
        height: 35vw;

        background-image: url("../assets/dept/large_chain.svg");
        background-repeat: no-repeat;
        background-size: 100% 100%;
        background-position: 50% 50%;
      }
      svg {
        position: absolute;

        top: 0;
        bottom: 0;
        margin: auto 0;
        left: 22vw;
      }
    }

    .dept_college {
      position: absolute;
      left: 70vw;
      top: 48vh;
      box-sizing: border-box;

      will-change: opacity, transform;
      transform: scale(0.55);
      p {
        position: absolute;
        left: 0;
        top: 0;
        width: 14vw;
        height: 4vh;
        text-align: center;
        vertical-align: bottom;
        background-color: #ffffff;
        border-radius: 20px;
        color: rgb(103, 192, 225);
        padding: 18px;
        font-size: 28px;
        letter-spacing: 4px;
        font-weight: 500;
        cursor: pointer;
      }
      p:hover {
        z-index: 999;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.3);
        width: 13vw;
        height: 5vh;
      }
      &.rotate-design > p:nth-child(1) {
        animation: rotate-30 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-design > p:nth-child(2) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-design > p:nth-child(3) {
        animation: rotate-150 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-design-disappear > p:nth-child(1) {
        animation: rotate-30-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-design-disappear > p:nth-child(2) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-design-disappear > p:nth-child(3) {
        animation: rotate-150-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-social > p:nth-child(1) {
        animation: rotate-23 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social > p:nth-child(2) {
        animation: rotate-68 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social > p:nth-child(3) {
        animation: rotate-113 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social > p:nth-child(4) {
        animation: rotate-158 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-social-disappear > p:nth-child(1) {
        animation: rotate-23-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social-disappear > p:nth-child(2) {
        animation: rotate-68-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social-disappear > p:nth-child(3) {
        animation: rotate-113-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-social-disappear > p:nth-child(4) {
        animation: rotate-158-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-undeclear > p {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-undeclear-disappear > p {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-engineer > p:nth-child(1) {
        animation: rotate-11 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(2) {
        animation: rotate-33 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(3) {
        animation: rotate-56 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(4) {
        animation: rotate-79 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(5) {
        animation: rotate-101 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(6) {
        animation: rotate-124 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(7) {
        animation: rotate-147 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(8) {
        animation: rotate-169 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      // &.rotate-engineer-2 > p:nth-child(1) {
      //   animation: rotate-15 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(2) {
      //   animation: rotate-45 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(3) {
      //   animation: rotate-75 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(4) {
      //   animation: rotate-105 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(5) {
      //   animation: rotate-135 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2 > p:nth-child(6) {
      //   animation: rotate-165 .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }

      &.rotate-engineer-disappear > p:nth-child(1) {
        animation: rotate-11-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(2) {
        animation: rotate-33-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(3) {
        animation: rotate-56-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(4) {
        animation: rotate-79-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(5) {
        animation: rotate-101-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(6) {
        animation: rotate-124-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(7) {
        animation: rotate-147-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(8) {
        animation: rotate-169-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      // &.rotate-engineer-2-disappear > p:nth-child(1) {
      //   animation: rotate-15-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(2) {
      //   animation: rotate-45-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(3) {
      //   animation: rotate-75-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(4) {
      //   animation: rotate-105-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(5) {
      //   animation: rotate-135-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }
      // &.rotate-engineer-2-disappear > p:nth-child(6) {
      //   animation: rotate-165-disappear .5s ease-in-out;
      //   animation-fill-mode: forwards;
      // }

      &.rotate-science > p:nth-child(1) {
        animation: rotate-18 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science > p:nth-child(2) {
        animation: rotate-54 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science > p:nth-child(3) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science > p:nth-child(4) {
        animation: rotate-126 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science > p:nth-child(5) {
        animation: rotate-162 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-science-disappear > p:nth-child(1) {
        animation: rotate-18-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science-disappear > p:nth-child(2) {
        animation: rotate-54-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science-disappear > p:nth-child(3) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science-disappear > p:nth-child(4) {
        animation: rotate-126-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-science-disappear > p:nth-child(5) {
        animation: rotate-162-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-humanity > p:nth-child(1) {
        animation: rotate-23 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity > p:nth-child(2) {
        animation: rotate-68 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity > p:nth-child(3) {
        animation: rotate-113 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity > p:nth-child(4) {
        animation: rotate-158 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-humanity-disappear > p:nth-child(1) {
        animation: rotate-23-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity-disappear > p:nth-child(2) {
        animation: rotate-68-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity-disappear > p:nth-child(3) {
        animation: rotate-113-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-humanity-disappear > p:nth-child(4) {
        animation: rotate-158-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-medicine > p:nth-child(1) {
        animation: rotate-15 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(2) {
        animation: rotate-45 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(3) {
        animation: rotate-75 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(4) {
        animation: rotate-105 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(5) {
        animation: rotate-135 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(6) {
        animation: rotate-165 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-medicine-disappear > p:nth-child(1) {
        animation: rotate-15-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(2) {
        animation: rotate-45-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(3) {
        animation: rotate-75-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(4) {
        animation: rotate-105-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(5) {
        animation: rotate-135-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(6) {
        animation: rotate-165-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-management > p:nth-child(1) {
        animation: rotate-18 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management > p:nth-child(2) {
        animation: rotate-54 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management > p:nth-child(3) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management > p:nth-child(4) {
        animation: rotate-126 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management > p:nth-child(5) {
        animation: rotate-162 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-management-disappear > p:nth-child(1) {
        animation: rotate-18-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management-disappear > p:nth-child(2) {
        animation: rotate-54-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management-disappear > p:nth-child(3) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management-disappear > p:nth-child(4) {
        animation: rotate-126-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-management-disappear > p:nth-child(5) {
        animation: rotate-162-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-computer > p:nth-child(1) {
        animation: rotate-45 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-computer > p:nth-child(2) {
        animation: rotate-135 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-computer-disappear > p:nth-child(1) {
        animation: rotate-45-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-computer-disappear > p:nth-child(2) {
        animation: rotate-135-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-biological > p:nth-child(1) {
        animation: rotate-45 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-biological > p:nth-child(2) {
        animation: rotate-135 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-biological-disappear > p:nth-child(1) {
        animation: rotate-45-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-biological-disappear > p:nth-child(2) {
        animation: rotate-135-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

    }

    .dept_list_section {
      grid-area: list;
      justify-self: flex-end;

      max-width: 22vw;

      ul {
        list-style-type: none;
        margin: 0;
        padding: 0;

        li {
          width: auto;
          margin: 0;
          padding: 0;

          transition-property: font-size, text-align, background-color, padding;
          transition-duration: .3s;
          transition-timing-function: ease;
        }

        li[data-key="design"]  {
          transform: translateX(14vw);
        }
        li[data-key="social"]  {
          transform: translateX(7vw);
        }
        li[data-key="undeclear"]  {
          transform: translateX(5vw);
        }
        li[data-key="engineer"]  {
          transform: translateX(4vw);
        }
        li[data-key="engineer-2"]  {
          transform: translateX(3.5vw);
        }
        li[data-key="science"]  {
          transform: translateX(3vw);
        }
        li[data-key="humanity"]  {
          transform: translateX(3.5vw);
        }
        li[data-key="medicine"]  {
          transform: translateX(4vw);
        }
        li[data-key="management"]  {
          transform: translateX(5vw);
        }
        li[data-key="computer"]  {
          transform: translateX(7vw);
        }
        li[data-key="biological"]  {
          transform: translateX(14vw);
        }
      }

      .dept_class_item {
        margin: 4vh;

        font-size: 1.3vw;

        p {
          width: 100%;
          margin: 0;
          padding: 0;
          color: rgb(150, 150, 150);
          text-align: left;
          border-radius: 10px;
          font-weight: 300;
          letter-spacing: 2px;
          cursor: pointer;
        }
        &.active {
            p {
              color: rgb(103, 192, 225);
              font-weight: bolder;
            }
            border-radius: 10px;
            padding: 3px;
          }
        &:hover {
          border-radius: 10px;
          background-color: #eeefef;
          padding: 3px;
          font-weight: 500;
        }
      }
    }
  }
</style>
