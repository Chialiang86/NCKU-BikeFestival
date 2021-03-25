<template lang="pug">
  div(class="dept_page")
    div(class="dept_top_bar_pc")
      div(class="dept_top_bar_layout" @click="scroll()")
        router-link(tag="label" class="dept_exit_button" to="/")
        div(class="dept_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===2")
          label(@click="openTab('hhttps://docs.google.com/forms/d/e/1FAIpQLSfx69xLr9XCqz6y8OEn4d8n6gc4qw3KzOn8FHb7Dm94pGwwmg/viewform'); list = false;" v-if="pc") 我要報名
    div(class="dept_top_bar_mobile" @click="deptList=false")
      div(class="dept_mobile_title" @click="list = false")
      router-link(tag="div" class="dept_mobile_exit_button" to="/")
      div(class="dept_mobile_list" @click="list = !list")
    div(class="dept_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://docs.google.com/forms/d/e/1FAIpQLSfx69xLr9XCqz6y8OEn4d8n6gc4qw3KzOn8FHb7Dm94pGwwmg/viewform'); list = false;" v-if="!pc") 我要報名
    div(class="dept_decoration_top")
    div(class="dept_decoration_bottom")
      //- g
        ellipse(cx="90%" cy="540" rx="50%" ry="100" stroke="#dddddd" stroke-width="5px" fill="none")
    div(class="dept_layout" v-show="mode===0" @click="list = false;")
      div(class="dept_select" v-show="!pc" @click="deptList=!deptList") {{tempClass}}
        div(class="dept_select_arrow")
      section(class="dept_list_section")
        div(class="dept_class" v-show="pc")
          ul
            li(v-for="(iter, index) in classes" v-bind:key="`${index}-${iter}`" v-bind:data-key="classKeys[index]" class="dept_class_item" v-on:click="chooseClass(index)" v-bind:class="{active: currentIndex === index}" )
              p {{iter}}
        div(class="dept_class_mobile" v-show="!pc && deptList")
          label(v-for="(iter, index) in classes" v-bind:key="`${index}-${iter}`" v-bind:data-key="classKeys[index]" class="dept_class_item" v-on:click="chooseClass(index); tempClass=iter; deptList=false;" v-bind:class="{active: currentIndex === index}" ) {{iter}}
      section(class="dept_chain_section" @click="deptList=false")
        div(id="small_chain" class="dept_chain_small")
        div(id="large_chain" class="dept_chain_large")
        div(id="large_chain_mobile" class="dept_chain_large_mobile")
        svg(width="56%" height="56%" viewBox="-10 -80 950 500")
          g(id="chain_path")
            path(d="M854.4800244140624,253.27999999999997 a66.84,66.84 0 0 0 0,-132.24 L218.7800244140625,2.7799999999999727 A187.68,187.68 0 0 0 186.50002441406252,0 c-103,0 -186.5,83.5 -186.5,186.5 s83.5,186.5 186.5,186.5 a187.68,187.68 0 0 0 32.25,-2.7800000000000002 z" fill="none" stroke-linejoin="round"  stroke-linecap="round" stroke-width="8px" stroke="#2DB8F5" stroke-dasharray="1, 20" stroke-miterlimit="10")
        div(v-for="(iter, index) in classKeys" v-bind:key="`${index}-${iter}-college`" class="dept_college" v-bind:class="deptBinding(index)" v-bind:style="showDeptCollege ? {} : { opacity: 1 }")
          p(v-for="(name, index) in colleges[iter]" v-bind:key="`${index}-${name}-college`" @click="tempClassKey=iter; tempDept=name; tempDeptFormal=collegesFormal[iter][index]; mode=1; showSlide();") {{name}}
    div(class="dept_layout_page" v-show="mode===1" @click="list = false;")
      div(class="return_section")
        div(class="back_icon" @click="mode=0") 回上頁
      div(class="list_section" v-show="pc")
        div(class="class_list_section")
          div(class="class_name") - {{classes[currentIndex]}} -
          div(class="class_list")
            div(id="temp_dept") {{tempDeptFormal}}
            label(v-for="(dept, index) in collegesFormal[tempClassKey]" v-show="dept!==tempDeptFormal" @click="tempDept=colleges[tempClassKey][index]; tempDeptFormal=collegesFormal[tempClassKey][index]; mode=1; showSlide();") {{dept}}
      div(id="dept_slide")
        img(v-for="(img, index) in deptSlides[tempDept]" v-bind:src="img")
        div(class="dept_empty")
</template>

<script>
import srcJson from '../assets//dept/dept.json'
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
      mode: 0,
      list: false,
      deptList: false,
      deptSlides: srcJson,
      tempClassKey: '',
      tempClass: '選擇學院',
      tempDept: '',
      tempDeptFormal: '',
      pc: this.isPC(),
      classes: ['規劃與設計學院', '社會科學院', '不分學院', '工學院', '理學院', '文學院', '醫學院', '管理學院', '電機資訊學院', '生物科學與科技學院'],
      classKeys: ['design', 'social', 'undeclear', 'engineer', 'science', 'humanity', 'medicine', 'management', 'computer', 'biological'],
      colleges: {
        design: ['建築', '都計', '工設'],
        social: ['政治', '經濟', '法律', '心理'],
        undeclear: ['不分系學程'],
        engineer: ['航太', '機械', '材料', '化工', '土木', '水利', '測量', '環工', '資源', '醫工', '能源', '工科', '系統'],
        science: ['數學', '化學', '物理', '地科', '光電'],
        humanity: ['中文', '外文', '歷史', '台文'],
        medicine: ['醫學', '牙醫', '藥學', '護理', '物治', '職治', '醫技'],
        management: ['會計', '統計', '企管', '交管', '工資管'],
        computer: ['資訊', '電機'],
        biological: ['生科', '生技']
      },
      collegesFormal: {
        design: ['建築學系', '都市計劃學系', '工業設計學系'],
        social: ['政治學系', '經濟學系', '法律學系', '心理學系'],
        undeclear: ['全校不分系學士學位學程'],
        engineer: ['航空太空工程學系', '機械工程學系', '材料科學及工程學系', '化學工程學系', '土木工程學系', '水利及海洋工程學系', '測量及空間資訊學系', '環境工程學系', '資源工程學系', '生物醫學工程學系', '能源國際學士學位學程', '工程科學系', '系統及船舶機電工程學系'],
        science: ['數學系', '化學系', '物理學系', '地球科學系', '光電科學與工程學系'],
        humanity: ['中國文學系', '外國語文學系', '歷史學系', '台灣文學系'],
        medicine: ['醫學系', '牙醫學系', '藥學系', '護理學系', '物理治療學系', '職能治療學系', '醫學檢驗生物技術學系'],
        management: ['會計學系', '統計學系', '企業管理學系', '交通管理科學系', '工業與資訊管理學系'],
        computer: ['資訊工程學系', '電機工程學系'],
        biological: ['生命科學系', '生物科技與產業科學系']
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
    showSlide: function () {
      var slide = document.getElementById('dept_slide')
      slide.classList.add('show-slide')
      const endEvent = function () {
        slide.classList.remove('show-slide')
        slide.removeEventListener('animationend', endEvent)
      }
      slide.addEventListener('animationend', endEvent)
    },
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

@keyframes show {
  0% {height: 0%;}
  100% {height: 100%;}
}

@keyframes scroll {
  0% {width: 0%;}
  100% {width: 92%;}
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
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-45deg) translateX(55vw) rotate(45deg); }
}
@keyframes rotate-135 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(45deg) translateX(55vw) rotate(-45deg); }
}
@keyframes rotate-45-disappear {
  0% { opacity: 1; transform: rotate(-45deg) translateX(55vw) rotate(45deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-135-disappear {
  0% { opacity: 1; transform: rotate(45deg) translateX(55vw) rotate(-45deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}

/* 3 components rotate*/
@keyframes rotate-30 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-60deg) translateX(55vw) rotate(60deg); }
}
@keyframes rotate-90 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(0deg) translateX(55vw) rotate(0deg); }
}
@keyframes rotate-150 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(60deg) translateX(55vw) rotate(-60deg); }
}

@keyframes rotate-30-disappear {
  0% { opacity: 1; transform: rotate(-60deg) translateX(55vw) rotate(60deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-90-disappear {
  0% { opacity: 1; transform: rotate(0deg) translateX(55vw) rotate(0deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-150-disappear {
  0% { opacity: 1; transform: rotate(60deg) translateX(55vw) rotate(-60deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}

/* 4 components roate*/
@keyframes rotate-23 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-67deg) translateX(55vw) rotate(67deg); }
}
@keyframes rotate-68 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-23deg) translateX(55vw) rotate(23deg); }
}
@keyframes rotate-113 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(23deg) translateX(55vw) rotate(-23deg); }
}
@keyframes rotate-158 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(68deg) translateX(55vw) rotate(-68deg); }
}

@keyframes rotate-23-disappear {
  0% { opacity: 1; transform: rotate(-67deg) translateX(55vw) rotate(67deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-68-disappear {
  0% { opacity: 1; transform: rotate(-23deg) translateX(55vw) rotate(23deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-113-disappear {
  0% { opacity: 1; transform: rotate(23deg) translateX(55vw) rotate(-23deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-158-disappear {
  0% { opacity: 1; transform: rotate(68deg) translateX(55vw) rotate(-68deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}

/* 5 components rotate */
@keyframes rotate-18 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-72deg) translateX(55vw) rotate(72deg); }
}
@keyframes rotate-54 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-36deg) translateX(55vw) rotate(36deg); }
}
/*
@keyframes rotate-90 {
  0% { transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { transform: rotate(0deg) translateX(55vw) rotate(0deg); }
}
*/
@keyframes rotate-126 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(36deg) translateX(55vw) rotate(-36deg); }
}
@keyframes rotate-162 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(72deg) translateX(55vw) rotate(-72deg); }
}

@keyframes rotate-18-disappear {
  0% { opacity: 1; transform: rotate(-72deg) translateX(55vw) rotate(72deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-54-disappear {
  0% { opacity: 1; transform: rotate(-36deg) translateX(55vw) rotate(36deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}

@keyframes rotate-126-disappear {
  0% { opacity: 1; transform: rotate(36deg) translateX(55vw) rotate(-36deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-162-disappear {
  0% { opacity: 1; transform: rotate(72deg) translateX(55vw) rotate(-72deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}

/* 6 components rotate */

@keyframes rotate-15 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-75deg) translateX(55vw) rotate(75deg); }
}
/*
@keyframes rotate-45 {
  0% { transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { transform: rotate(-45deg) translateX(55vw) rotate(45deg); }
}
*/
@keyframes rotate-75 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-15deg) translateX(55vw) rotate(15deg); }
}
@keyframes rotate-105 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(15deg) translateX(55vw) rotate(-15deg); }
}
/*
@keyframes rotate-135 {
  0% { transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { transform: rotate(45deg) translateX(55vw) rotate(-45deg); }
}
*/
@keyframes rotate-165 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(75deg) translateX(55vw) rotate(-75deg); }
}

@keyframes rotate-15-disappear {
  0% { opacity: 1; transform: rotate(-75deg) translateX(55vw) rotate(75deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}

@keyframes rotate-75-disappear {
  0% { opacity: 1; transform: rotate(-15deg) translateX(55vw) rotate(15deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-105-disappear {
  0% { opacity: 1; transform: rotate(15deg) translateX(55vw) rotate(-15deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}

@keyframes rotate-165-disappear {
  0% { opacity: 1; transform: rotate(75deg) translateX(55vw) rotate(-75deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}

/* 7 components rotate */

@keyframes rotate-12 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-78deg) translateX(55vw) rotate(78deg); }
}
@keyframes rotate-37 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-53deg) translateX(55vw) rotate(53deg); }
}
@keyframes rotate-63 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-27deg) translateX(55vw) rotate(27deg); }
}

@keyframes rotate-117 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(27deg) translateX(55vw) rotate(-27deg); }
}
@keyframes rotate-143 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(53deg) translateX(55vw) rotate(-53deg); }
}
@keyframes rotate-168 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(78deg) translateX(55vw) rotate(-78deg); }
}

@keyframes rotate-12-disappear {
  0% { opacity: 1; transform: rotate(-78deg) translateX(55vw) rotate(78deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-37-disappear {
  0% { opacity: 1; transform: rotate(-53deg) translateX(55vw) rotate(53deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-63-disappear {
  0% { opacity: 1; transform: rotate(-27deg) translateX(55vw) rotate(27deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-117-disappear {
  0% { opacity: 1; transform: rotate(27deg) translateX(55vw) rotate(-27deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-143-disappear {
  0% { opacity: 1; transform: rotate(53deg) translateX(55vw) rotate(-53deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-168-disappear {
  0% { opacity: 1; transform: rotate(78deg) translateX(55vw) rotate(-78deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}

/* 13 components rotate */

@keyframes rotate-1 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(56vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-85deg) translateX(56vw) rotate(85deg); }
}
@keyframes rotate-25 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-63deg) translateX(55vw) rotate(63deg); }
}
@keyframes rotate-44 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-47deg) translateX(55vw) rotate(47deg); }
}
@keyframes rotate-59 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-34deg) translateX(55vw) rotate(34deg); }
}
@keyframes rotate-71 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-22deg) translateX(55vw) rotate(22deg); }
}
@keyframes rotate-81 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(-11deg) translateX(55vw) rotate(11deg); }
}

@keyframes rotate-99 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(11deg) translateX(55vw) rotate(-11deg); }
}
@keyframes rotate-109 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(22deg) translateX(55vw) rotate(-22deg); }
}
@keyframes rotate-121 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(34deg) translateX(55vw) rotate(-34deg); }
}
@keyframes rotate-136 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(47deg) translateX(55vw) rotate(-47deg); }
}
@keyframes rotate-155 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(55vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(63deg) translateX(55vw) rotate(-63deg); }
}
@keyframes rotate-179 {
  0% { opacity: 0; transform: rotate(-90deg) translateX(56vw) rotate(90deg); }
  100% { opacity: 1; transform: rotate(85deg) translateX(56vw) rotate(-85deg); }
}

@keyframes rotate-1-disappear {
  0% { opacity: 1; transform: rotate(-85deg) translateX(56vw) rotate(85deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(56vw) rotate(-90deg); }
}
@keyframes rotate-25-disappear {
  0% { opacity: 1; transform: rotate(-63deg) translateX(55vw) rotate(63deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-44-disappear {
  0% { opacity: 1; transform: rotate(-47deg) translateX(55vw) rotate(47deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-59-disappear {
  0% { opacity: 1; transform: rotate(-34deg) translateX(55vw) rotate(34deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-71-disappear {
  0% { opacity: 1; transform: rotate(-22deg) translateX(55vw) rotate(22deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-81-disappear {
  0% { opacity: 1; transform: rotate(-11deg) translateX(55vw) rotate(11deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-99-disappear {
  0% { opacity: 1; transform: rotate(11deg) translateX(55vw) rotate(-11deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-109-disappear {
  0% { opacity: 1; transform: rotate(22deg) translateX(55vw) rotate(-22deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-121-disappear {
  0% { opacity: 1; transform: rotate(34deg) translateX(55vw) rotate(-34deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-136-disappear {
  0% { opacity: 1; transform: rotate(47deg) translateX(55vw) rotate(-47deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-155-disappear {
  0% { opacity: 1; transform: rotate(63deg) translateX(55vw) rotate(-63deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(55vw) rotate(-90deg); }
}
@keyframes rotate-179-disappear {
  0% { opacity: 1; transform: rotate(85deg) translateX(56vw) rotate(-85deg); }
  100% { opacity: 0; transform: rotate(90deg) translateX(56vw) rotate(-90deg); }
}

  /*
    phone layout css
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
      .dept_mobile_title {
        grid-area: title;
        width: 60vw;
        background-image: url('../assets//dept/title.svg');
        background-repeat: no-repeat;
        background-size: 75% 75%;
        background-position: center center;
      }
      .dept_mobile_list {
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
    .dept_mobile_list_area {
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
        left: -155vw;

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
        width: 22vw;
        height: 2.6vh;
        text-align: center;
        vertical-align: bottom;
        background-color: rgb(44,185,244);
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
        animation: rotate-1 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(2) {
        animation: rotate-25 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(3) {
        animation: rotate-44 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(4) {
        animation: rotate-59 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(5) {
        animation: rotate-71 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(6) {
        animation: rotate-81 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(7) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(8) {
        animation: rotate-99 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(9) {
        animation: rotate-109 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(10) {
        animation: rotate-121 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(11) {
        animation: rotate-136 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(12) {
        animation: rotate-155 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(13) {
        animation: rotate-179 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-engineer-disappear > p:nth-child(1) {
        animation: rotate-1-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(2) {
        animation: rotate-25-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(3) {
        animation: rotate-44-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(4) {
        animation: rotate-59-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(5) {
        animation: rotate-71-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(6) {
        animation: rotate-81-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(7) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(8) {
        animation: rotate-99-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(9) {
        animation: rotate-109-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(10) {
        animation: rotate-121-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(11) {
        animation: rotate-136-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(12) {
        animation: rotate-155-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(13) {
        animation: rotate-179-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

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
        animation: rotate-12 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(2) {
        animation: rotate-37 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(3) {
        animation: rotate-63 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(4) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(5) {
        animation: rotate-117 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(6) {
        animation: rotate-143 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(7) {
        animation: rotate-168 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-medicine-disappear > p:nth-child(1) {
        animation: rotate-12-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(2) {
        animation: rotate-37-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(3) {
        animation: rotate-63-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(4) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(5) {
        animation: rotate-117-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(6) {
        animation: rotate-143-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(7) {
        animation: rotate-168-disappear .5s ease-in-out;
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

    .dept_select {
      z-index:50;
      position: absolute;
      left: 2vw;
      top: 10vh;
      width: 52vw;
      padding: 1vh 0 1vh 4vw;
      height: 3vh;
      // border: 1px solid rgb(180, 180, 180);
      background-color: rgb(131,218,242);
      color: white;
      text-align: left;
      line-height: 3vh;
      font-size: 2vh;
      font-weight: bold;
      letter-spacing: 1px;
      border-radius: 10px;
      box-shadow: 1px 1px 2px 1px rgb(150, 150, 150);

      .dept_select_arrow {
        position: absolute;
        padding: 1vh;
        margin: 0.5vh;
        right: 0;
        top: 0;
        width: 2vh;
        height: 2vh;
        transform: rotate(90deg);
        background-image: url('../assets//arrow.svg');
        background-size: 60% 60%;
        background-repeat: no-repeat;
        background-position: center center;
        border-radius: 5px;
        &:hover {
          filter: brightness(110%);
        }
        &:active {
          filter: brightness(60%);
          background-color: rgba(255, 255, 255, 0.4);
        }
      }
    }

    .dept_list_section {
      z-index:50;
      position: absolute;
      left: 2vw;
      top: 16vh;
      .dept_class_mobile {
        height: auto;
        display: flex;
        flex-direction: column;
        label {
          padding: 1vh 1vw;
          height: 3vh;
          width: 54vw;
          border: 1px solid rgb(254,241,217);
          font-weight: bold;
          letter-spacing: 1px;
          background-color: rgb(131,218,242);
          // background-color: rgb(254,241,217);
          color: white;
          line-height: 3vh;
          font-size: 2vh;
        }
      }
    }
    .dept_layout_page {
      width: 100vw;
      height: 100vh;
      display: grid;
      grid-template-rows: 8vh 8vh 1fr;
      grid-template-areas: "." "return" "slide";
      justify-content: center;
      align-items: flex-start;
    }
    .return_section {
      grid-area: return;
      width: 100vw;
      height: 100%;
      display: flex;
      justify-content: flex-end;
      align-items: center;
      .back_icon {
        margin: 2vh 5vw;
        width: 26vw;
        height: 4vh;
        background-color: rgb(131,218,242);
        color: white;
        font-size: 2.5vh;
        font-weight: bold;
        line-height: 4vh;
        letter-spacing: 2px;
        border-radius: 10px;
        // background-image: url('../assets//return.svg');
        &:hover {
          filter: brightness(110%);
        }
        &:active {
          filter: brightness(60%);
        }
      }
    }
    #dept_slide {
      grid-area: slide;
      width: 100vw;
      height: 100%;
      overflow-x: hidden;
      overflow-y: scroll;

      display: flex;
      flex-direction: column;
      img {
        padding: 0 1vw 1vh 1vw;
        width: 98vw;
      }
      .dept_empty {
        width: 98vw;
        padding: 10vh 0;
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
      grid-template-rows: 6vh 1fr;
      grid-template-areas: ". ." "list chain";

      justify-content: center;
      justify-items: center;
      align-items: center;

      width: 100vw;
      min-width: 1000px;
      height: 100vh;
    }

    .dept_chain_section {
      grid-area: chain;

      display: grid;
      grid-template-columns: 4vw repeat(5, 1fr);
      grid-template-rows: 4vw repeat(3, 1fr) 6vw;
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

        left: 52vw;
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
      top: 50vh;
      box-sizing: border-box;

      will-change: opacity, transform;
      transform: scale(0.36);
      p {
        position: absolute;
        left: 0;
        top: 0;
        width: 18vw;
        height: 5vh;
        text-align: center;
        vertical-align: bottom;
        background-color: #ffffff;
        border-radius: 20px;
        color: rgb(103, 192, 225);
        padding: 18px;
        font-size: 5vh;
        line-height: 5vh;
        letter-spacing: 4px;
        font-weight: 500;
        cursor: pointer;
        z-index: 100;
      }
      p:hover {
        z-index: 999;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.3);
        width: 18vw;
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
        animation: rotate-1 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(2) {
        animation: rotate-25 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(3) {
        animation: rotate-44 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(4) {
        animation: rotate-59 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(5) {
        animation: rotate-71 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(6) {
        animation: rotate-81 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(7) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(8) {
        animation: rotate-99 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(9) {
        animation: rotate-109 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(10) {
        animation: rotate-121 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(11) {
        animation: rotate-136 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(12) {
        animation: rotate-155 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer > p:nth-child(13) {
        animation: rotate-179 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-engineer-disappear > p:nth-child(1) {
        animation: rotate-1-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(2) {
        animation: rotate-25-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(3) {
        animation: rotate-44-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(4) {
        animation: rotate-59-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(5) {
        animation: rotate-71-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(6) {
        animation: rotate-81-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(7) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(8) {
        animation: rotate-99-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(9) {
        animation: rotate-109-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(10) {
        animation: rotate-121-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(11) {
        animation: rotate-136-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(12) {
        animation: rotate-155-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-engineer-disappear > p:nth-child(13) {
        animation: rotate-179-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }

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
        animation: rotate-12 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(2) {
        animation: rotate-37 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(3) {
        animation: rotate-63 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(4) {
        animation: rotate-90 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(5) {
        animation: rotate-117 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(6) {
        animation: rotate-143 .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine > p:nth-child(7) {
        animation: rotate-168 .5s ease-in-out;
        animation-fill-mode: forwards;
      }

      &.rotate-medicine-disappear > p:nth-child(1) {
        animation: rotate-12-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(2) {
        animation: rotate-37-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(3) {
        animation: rotate-63-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(4) {
        animation: rotate-90-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(5) {
        animation: rotate-117-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(6) {
        animation: rotate-143-disappear .5s ease-in-out;
        animation-fill-mode: forwards;
      }
      &.rotate-medicine-disappear > p:nth-child(7) {
        animation: rotate-168-disappear .5s ease-in-out;
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

      max-width: 24vw;

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
          transform: translateX(10vw);
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
          transform: translateX(10vw);
        }
      }

      .dept_class_item {
        margin: 4vh;

        font-size: 1.3vw;
        z-index: 100;

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
          font-size: 1.4vw;
          }
        &:hover {
          border-radius: 10px;
          background-color: #eeefef;
          font-size: 1.4vw;
          font-weight: 500;
        }
      }
    }
    .dept_layout_page {
      width: 100vw;
      height: 100vh;
      display: grid;
      grid-template-rows: 10vh 1fr;
      grid-template-columns: 32vw 1fr 13vw;
      grid-template-areas: ". . ." "list slide return";
      justify-content: center;
      justify-items: center;
      align-items: flex-start;
    }
    .return_section {
      grid-area: return;
      z-index: 50;
      .back_icon {
        margin: 1vh 1vw;
        width: 8vw;
        height: 5vh;
        background-color: rgb(131,218,242);
        color: white;
        font-size: 3vh;
        font-weight: bold;
        line-height: 4.5vh;
        letter-spacing: 2px;
        border-radius: 5px;
        background-repeat: no-repeat;
        background-position: center center;
        border-radius: 10px;
        &:hover {
          cursor: pointer;
          transform: scale(1.1);
          filter: brightness(110%);
        }
        &:active {
          filter: brightness(60%);
        }
      }
    }
    .list_section {
      grid-area: list;
      width: 92%;
      height: 100%;
      z-index: 50;
      background-image: url('../assets//main_blue.svg');
      background-repeat: no-repeat;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      animation: show .6s ease;
      .class_list_section {
        margin: 2vh 1vw;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
        z-index: 50;
        .class_name {
          width: 100%;
          margin: 1vh 0 2vh 0;
          padding: 3vh 1vw;
          line-height: 4vh;
          font-size: 4vh;
          font-weight: bold;
          letter-spacing: 4px;
          color: white;
        }
        .class_list {
          height: 65vh;
          display: flex;
          flex-direction: column;
          justify-content: flex-start;
          align-items: center;
          overflow-y: scroll;
          overflow-x: hidden;
          &::-webkit-scrollbar {
            width: 0.3vw;
            border-radius: 0.5vw;
          }
          &::-webkit-scrollbar-track {
            background: transparent;
            border-radius: 0.5vw;
          }
          &::-webkit-scrollbar-thumb {
            background: white;
            border-radius: 0.5vw;
          }
          #temp_dept {
            margin: 0 0 1vh 0;
            padding: 2vh 1vw;
            color:white;
            line-height: 3.2vh;
            font-size: 3.2vh;
            font-weight: bold;
            letter-spacing: 2px;
            background-color: rgba(255, 255, 255, 0.4);
            width: 24vw;
            height: 4vh;
          }
          label {
            margin: 0 0 1vh 0;
            padding: 2vh 1vw;
            color:white;
            line-height: 3vh;
            font-size: 3vh;
            letter-spacing: 2px;
            width: 24vw;
            height: 4vh;
            &:hover {
              background-color: rgba(255, 255, 255, 0.2);
            }
            &:active {
              filter: brightness(60%);
            }
          }
        }
      }
    }
    #dept_slide {
      grid-area: slide;
      width: 100%;
      height: 100%;
      overflow-x: hidden;
      overflow-y: scroll;
      z-index: 50;
      // background-color: rgb(255, 255, 255);
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      align-items: center;
      &::-webkit-scrollbar {
        width: 0.5vw;
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
      img {
        padding: 0 0 1vh 0;
        width: 98%;
      }
    }
    .show-slide {
      animation: show .6s ease;
    }
  }
</style>
