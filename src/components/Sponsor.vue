<template lang="pug">
  div(class="sponsor_page")
    //div(class="sponsor_title")
    div(class="live_top_bar_pc")
      div(class="live_top_bar_layout" @click="scroll()")
        router-link(tag="label" class="live_exit_button" to="/")
        div(class="live_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===5")
          label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="pc") 我要報名
    div(class="live_top_bar_mobile")
      div(class="live_mobile_title" @click="list = false")
      router-link(tag="div" class="live_mobile_exit_button" to="/")
      div(class="live_mobile_list" @click="list = !list")
    div(class="live_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://reurl.cc/pmZKrx'); list = false;" v-if="pc") 我要報名
    button(v-show="mode === 1" class="sponsor_return_button" v-on:click="mode = 0; currentIndex = -1;")
    div(class="sponsor_layout_1" v-show="mode === 0")
      section(class="sponsor_list")
        label(v-for="(iter, index) of sponsor" v-bind:key="iter.name" v-bind:href="`${iter.link}`" target="_blank" v-bind:data-name="iter.name" v-bind:style="{'background-image': 'url(' + `/static/sponsor_${index + 1}.jpg` + ')'}" v-on:click="currentIndex = index; mode = 1;")
    div(class="sponsor_layout_2" v-if="mode === 1")
      section(class="sponsor_info")
        p {{sponsor[currentIndex].name}}
        label(v-bind:style="{'background-image': 'url(' + `/static/sponsor_${currentIndex + 1}.jpg` + ')'}" v-on:click="openSponsorTab(currentIndex)")
      section(class="sponsor_content")
        article(v-html="sponsor[currentIndex].content")

</template>

<script>
export default {
  data: function () {
    return {
      menuText: ['最新消息', '活動介紹', '科系概覽', '線上資源', '家長專欄', '合作單位', '直播專區'],
      urlText: ['news', 'activity', 'department', 'online', 'parent', 'sponsor', 'live'],
      list: false,
      pc: this.isPC(),
      currentIndex: -1,
      mode: 0,
      sponsor: [
        {
          name: '悠遊卡公司',
          link: 'https://www.easycard.com.tw/',
          content: `<p align="center">
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">悠遊卡成立將近20年，發卡量超過9千萬張，流通卡也突破7千萬張，可以使用的支付點遍佈全台突破7萬處，讓民眾在食、衣、住、行、育、樂各方面，都能享受最便利的支付體驗，是我們持續努力的目標。我們秉持便利、安全、信任、公益的核心價值，追求最領先的行動支付技術，與大家一同邁向未來無現金的美好生活！</span></span></span></p>
<p><img src="https://i.imgur.com/yfKiNzl.jpg") style="border:2px white solid;padding:5px;text-align:center; height:35vh;"></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">官網</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://www.easycard.com.tw/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://www.easycard.com.tw/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">粉專</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://www.facebook.com/easycard.tw/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://www.facebook.com/easycard.tw/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">台北市南港區園區街3-1號13F</span></span></span></p>`
        },
        {
          name: '萬鼎工程',
          link: 'http://www.res.com.tw/',
          content: `<p align="center">
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">萬鼎工程服務股份有限公司為CTCI中鼎集團成員，成立於1984年。萬鼎的工程經驗豐富，深度了解業主需求及當地環境特色，為土地營造新願景。
萬鼎工程是工程統包之領導廠商，提供公共工程統包解決方案，尤其在土木建築和交通工程領域累積豐富的工程經驗和技術。服務內容涵蓋工程規劃、設計、興建、測試、驗證，並專精冰凍工法以及智能頂進工法。
</span></span></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">萬鼎工程積極經營建築、交通、大地、潛盾、水利、環境、電機、土地開發等市場，穩居台灣統包工程產業領先地位。
</span></span></span></p>
<p><img src="https://imgur.com/GPmjmUW.jpg") style="border:2px white solid;padding:5px;text-align:center; height:35vh;"></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">官網</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="http://www.res.com.tw/"><span style="font-family: GenYoGothicTW-Bold, serif;">http://www.res.com.tw/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">台北辦公室-地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">台北市南港路三段48號4樓</span></span></span></span>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">高雄辦公室-地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">高雄市橋頭區成功路163號</span></span></span></span></p>`
        },
        {
          name: '丹莉蛋捲',
          link: 'https://www.danli.com.tw/',
          content: `<p align="center">
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">「丹莉手作蛋捲」的原料特別挑選新鮮雞蛋健康植物油，少糖少油比例，不添加一滴水，精心調配而成。不添加奶粉、奶精、防腐劑、膨鬆劑、酵素，讓您每口都能安心享用~品嚐
厚工捲：要說是山珍海味，是一點也不為過，採用頂級海苔片，搭配嚴選豬肉鬆，層層搭在蛋捲裏。說多香就有多香，要鮮美就多鮮美。製作真厚工，但我們相信堅持就會是美味。
</span></span></span></p>
<p><img src="https://imgur.com/64lfxDo.jpg") style="border:2px white solid;padding:5px;text-align:center; height:35vh;"></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">官網</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://www.danli.com.tw/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://www.danli.com.tw/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">粉專</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://www.facebook.com/danlieggrolls"><span style="font-family: GenYoGothicTW-Bold, serif;">https://www.facebook.com/danlieggrolls</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">台南市永康區鹽行路67號</span></span></span></p>`
        },
        {
          name: '方蘭川布丁',
          link: 'https://fanglanchuan.tw/',
          content: `<p align="center">
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">只有一種口味的焦皮布丁專賣店</span></span></span></p>
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">不以鮮奶油來取代鮮奶的角色，少掉了一些香氣，卻換來更多的健康與最初的感動，而扎實的口感與多層次的表現，就是“焦皮布丁”的主要特色。</span></span></span></p>
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">方蘭川布丁能夠讓您在送禮時，充滿面子的原因很簡單，當坊間在追求(價格)時，我們發自內心所重視的卻是【價值】</span></span></span></p>
<p><img src="https://imgur.com/mVLVN5S.jpg") style="border:2px white solid;padding:5px;text-align:center; height:35vh;"></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">官網</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://fanglanchuan.tw/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://fanglanchuan.tw/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">粉專</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://www.facebook.com/flcpudding/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://www.facebook.com/flcpudding/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">台南市安平區安北路112號</span></span></span></p>`
        },
        {
          name: '自助家StudyDIY',
          link: 'https://study-diy.com.tw/',
          content: `<p align="center">
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">自助家 StudyDIY - 屬於自己的歐美遊學STYLE</span></span></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">在講求客製化服務的時代，公式化遊學行程已經OUT！何況歐美國家遊學一趟所費不貲，錢不花得開心滿意怎麼行？自助家遊學的專業顧問將會針對你的個性、經歷與目標，為你規劃一趟真正符合個人需求和興趣的遊學行程。</span></span></span></p>
<p><img src="https://study-diy.com.tw/assets/about_img3-f8c0eba2b6ad72cfbe61c3ffe3fd39632f6827f43432f0d2c51d89cd18a67f38.jpg") style="border:2px white solid;padding:5px;text-align:center; height:35vh;"></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">官網</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://study-diy.com.tw/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://study-diy.com.tw/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">粉專</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://www.facebook.com/StudyDIY/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://www.facebook.com/StudyDIY/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">台南市民族路二段76-10號6樓</span></span></span></p>`
        },
        {
          name: '非凡遊學',
          link: 'https://feifanstudy.com/',
          content: `<p align="center">
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">非凡 Feifan Study - 小資也能圓夢 菲律賓遊學</span></span></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">
跟隨日韓的腳步，近年在台灣越來越火熱的菲律賓遊學，主打可密集可輕鬆的課程安排、平價的一對一專業教學，你是不是也從朋友的朋友那邊聽說過呢？非凡遊學目前和超過60間菲律賓語言學校合作，我們相信「沒有最好的學校，只有最適合你的學校」，從你的角度出發，提供最實用完整的建議！</span></span></span></p>
<p><img src="https://imgur.com/V3pqo1V.jpg") style="border:2px white solid;padding:5px;text-align:center; height:35vh;"></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">官網</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://feifanstudy.com/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://feifanstudy.com/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">粉專</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://www.facebook.com/FeiFanStudy/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://www.facebook.com/FeiFanStudy/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">台北市大安區忠孝東路四段2號</span></p>`
        },
        {
          name: '晴天包子',
          link: 'http://www.ssbuns.com.tw/',
          content: `<p align="center">
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">晴天包子創立於2012年，坐落於台南市東區文化中心及巴克禮公園旁。以製作高品質中式發酵麵食為目標，期初即以現代化門市販售商品。多年來，不靠廣告、媒體採訪，不靠網路行銷。僅僅以創造美好產品的衷心，成功擄獲台南顧客的胃。晴天包子單店，30種產品，平均一年可服務超過20萬組顧客，銷售達160萬個以上的產品。我們衷心期待大家的光臨，來台南品嘗這單純、且獨一無二的美好。</span></span></span></p>
<p><img src="https://imgur.com/UDLc2WB.jpg") style="border:2px white solid;padding:5px;text-align:center; height:35vh;"></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">官網</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="http://www.ssbuns.com.tw/"><span style="font-family: GenYoGothicTW-Bold, serif;">http://www.ssbuns.com.tw/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">台南市東區中華東路三段355號 (巴克禮公園旁)</span></p>`
        },
        {
          name: '新飛遊學',
          link: 'https://www.hsinfei.com/',
          content: `<p align="center">
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">新飛在菲律賓遊學領域，不只是菲律賓遊學代辦，而是自詡為學員的全方位海外英語學習協助顧問，從一開始的諮詢、到中間的代辦過程、及出發後的在地照顧，皆一應俱全。且這中間過程新飛不會向學員收取代辦費，因為新飛做為菲律賓語言學校在台灣的代理，是由學校會給予新飛合理的合作費用。</span></span></span></p>
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">最後，希望能夠藉由分享新飛國際的經驗，讓您也能一同體會這份感動，了解到學英文其實是可以很快樂、很有效率的。然後藉由這段過程，提升台灣競爭力，增加國際觀，成為遨遊世界通行無阻的菁英！</span></span></span></p>
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">Hsinfei Always Touches Your Heart!</span></span></span></p>
<p><img src="https://imgur.com/z73X0Vd.jpg") style="border:2px white solid;padding:5px;text-align:center; height:35vh;"></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">官網</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://www.hsinfei.com/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://www.hsinfei.com/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">台南市中西區民生路一段167號3F</span></p>`
        },
        {
          name: '川益科技股份有限公司',
          link: 'https://www.kingslide.com/',
          content: `<p align="center">
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;"> 於2006年成立在高雄科學園區內的川益科技，以創造消費需求而創立， 結合ID設計元素及專業研發人才研究與開發頂尖的先進製程技術與材料應用並投入電子零組件與消費性電子產品產業， 運用多種嶄新技術研發製造出精度高、觸感佳、質感優的金屬配件，成功引領3C產品朝向多樣化與個性化的方向發展。</span></span></span></p>
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">卓越的研發技術與優良品質使King Slide品牌不僅在雲端產業與高階廚具產業獲得業界翹楚之指定品牌，在邁入消費性電子產品的領域發展之際， King Slide功能性機構產品已朝向全方位應用端發展，現已成為各高科技大廠及各行業翹楚指名的長期合作伙伴。</span></span></span></p>
<p><a name="_GoBack"></a> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">Hsinfei Always Touches Your Heart!</span></span></span></p>
<p><img src="https://i.imgur.com/0tBwKXD.png") style="border:2px white solid;padding:5px;text-align:center; height:35vh;"></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">官網</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="color: #0563c1;"><u><a href="https://www.hsinfei.com/"><span style="font-family: GenYoGothicTW-Bold, serif;">https://www.kingslide.com/</span></a></u></span></p>
<p><span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="font-family: GenYoGothicTW-Bold;">地址</span></span></span><span style="font-family: GenYoGothicTW-Bold, serif;">:</span> <span style="font-family: GenYoGothicTW-Bold;"><span lang="zh-TW"><span style="color: #1d2129;"><span style="font-family: GenYoGothicTW-Bold;"><span style="font-size: medium;">台灣高雄市路竹區路科九路6號</span></p>`
        }
      ]
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
    openSponsorTab: function (index) {
      window.open(this.sponsor[index].link)
    }
  }
}
</script>

<style lang="scss" scoped>
  @keyframes mouse {
    from {
      transform: translateX(2vw) translateY(2vw);
    }
    to {
      transform: translateX(0vw) translateY(0vw);
    }
  }
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
    .sponsor_page {
      display: flex;
      align-items: center;
      justify-content: center;
      // position: relative;
      width: 100vw;
      height: 100vh;
      margin: 0;
      padding: 0;
      background: rgb(255, 246, 232);
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
        background-image: url('../assets/14/sponsor/title.svg');
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

    .sponsor_title {
      position: absolute;
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 20;
      top: 3vh;

      width: 45vw;
      height: 20vw;
      background-image: url("../assets/14/sponsor/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: center top 10%;
    }

    .sponsor_return_button {
      position: absolute;
      right: 7vw;
      top: 7vw;

      width: 7.5vw;
      height: 7.5vw;
      background-color: transparent;
      background-image: url("../assets/return.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: 50% 50%;

      outline: none;
      border: none;
      filter: brightness(120%);

      cursor: pointer;

      transition: filter .3s ease;

      &:hover {
        filter: brightness(130%);
      }
      &:active {
        filter: brightness(80%);
      }
    }

    .sponsor_layout_1 {
      display: grid;
      grid-template-rows: 1fr 3fr;
      grid-template-areas: "top"
        "bottom";
      justify-content: center;
      align-content: bottom;
      align-items: bottom;

      background-color: rgb(255, 246, 232);
      width: 100%;
      min-height: 50vh;
      overflow-y: scroll;
    }

    .sponsor_layout_2 {
      display: grid;
      grid-template-rows: 1fr 3fr;
      grid-template-areas: "top bottom";
      justify-content: center;
      align-content: center;
      align-items: flex-start;

      background-color: rgb(255, 246, 232);
      width: 100vw;
      height: 92vh;
      top: 8%;
      overflow-y: scroll;
    }

    .sponsor_list {
      grid-area: bottom;

      display: grid;
      grid-template-rows: 60vw 60vw;
      grid-gap: 10vw;
      justify-content: center;

      width: 100vw;
      height: 85vh;
      overflow-y: scroll;

      label {
        width: 60vw;
        height: 60vw;
        background-position: 50% 50%;
        background-size: contain;
        background-repeat: no-repeat;
        text-align: center;
        transition: .2s transform ease-in-out;
        cursor: pointer;
        &:hover {
          transform: scale(1.1);
          &:before {
            position: absolute;
            display: block;
            content: '';
            width: 60vw;
            height: 60vw;
            background-color: rgba(0, 0, 0, 0.66);
            z-index: 20;
          }
          &:after {
            position: absolute;
            content: attr(data-name);
            color: white;
            margin-top: 30vw;
            margin-left: -30vw;
            width: 60vw;
            z-index: 21;
          }
        }
      }
    }
    .sponsor_info {
      display: none;
    }
    .sponsor_content {
      grid-area: bottom;

      display: grid;
      grid-template-rows: 1fr 70vh 1fr;
      grid-template-areas: "."
        "content"
        "ncku";
      justify-content: center;
      justify-items: center;
      align-content: center;
      align-items: center;

      width: 100%;
      min-height: 100vh;
      height: 100%;

      p {
        grid-area: ncku;
        line-height: 20px;
        color: rgb(103, 192, 225);
        font-size: 1.8vh;
      }

      article {
        text-align: left;
        grid-area: content;
        line-height:3vh;

        width: 90%;
        height: 70vh;
        overflow: scroll;
      }
    }

  }

  /*
    computer layout css
  */
  @media only screen and (min-width: 552px) {    @font-face {
    font-family: 'GenYoGothicTW-Bold';
    src: url('../assets/fonts/GenYoGothicTW-Bold.woff') format("woff"),
          url('../assets/fonts/GenYoGothicTW-Bold.ttf') format("truetype"),
          url('../assets/fonts/GenYoGothicTW-Bold.eot') format("embedded-opentype");
    }
    @import url('https://fonts.googleapis.com/css?family=Noto+Sans+TC');
    *{
      font-family: 'Noto Sans TC'!important;
    }
    .sponsor_page {
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

    .sponsor_title {
      position: absolute;
      z-index: 20;
      left: 4vw;
      top: 6vh;

      width: 17vw;
      height: 8vw;
      background-image: url("../assets/14/sponsor/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: 50% 50%;
    }

    .sponsor_return_button {
      position: absolute;
      z-index: 1;
      left: 2vw;
      top: 7vw;

      width: 3vw;
      height: 3vw;
      background-color: transparent;
      background-image: url("../assets/return.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: 50% 50%;

      outline: none;
      border: none;

      cursor: pointer;

      transition: filter .3s ease;

      &:hover {
        filter: brightness(110%);
      }
      &:active {
        filter: brightness(80%);
      }
    }

    .sponsor_layout_1 {
      display: grid;
      grid-template-columns: 1fr 5fr 1fr;
      grid-template-rows: 1fr 1.8fr 0.8fr;
      grid-template-areas: ". . ."
        ". list ."
        ". . .";
      justify-content: center;
      align-content: center;
      align-items: center;

      width: 100vw;
      height: 100vh;

      background-color: rgb(255, 246, 232);
    }

    .sponsor_layout_2 {
      display: grid;
      position: absolute;
      grid-template-columns: 1fr 1.5fr;
      grid-template-areas: "left right";
      width: 100vw;
      height: 92vh;
      top: 8%;
    }

    .sponsor_list {
      grid-area: list;

      display: grid;
      grid-template-columns: repeat(8, 7vw);
      grid-template-rows: repeat(3, 7vw);
      grid-gap: 3vw;

      width: 100%;
      label {
        width: 7vw;
        height: 7vw;
        background-position: 50% 50%;
        background-size: contain;
        background-color : white;
        background-repeat: no-repeat;

        transition: .2s transform ease-in-out;
        cursor: pointer;
        &:hover {
          transform: scale(1.1);
          &:before {
            position:absolute;
            display: block;
            text-align: center;
            content: '';
            // text-align: top -20%;
            width: 7vw;
            height: 7vw;
            background-color: rgba(0, 0, 0, 0.66);
            z-index: 20;
          }
          &:after {
            position:absolute;
            content: attr(data-name);
            color: white;
            text-align: center;
            // text-align: top -20%;
            margin-top: 2vh;
            margin-left: -3.5vw;
            width: 7vw;
            z-index: 21;
          }
        }
      }
    }

    .sponsor_info {
      grid-area: left;

      display: grid;
      grid-template-rows: 1fr 1fr 2fr 1fr;
      grid-template-areas: "."
        "title"
        "icon"
        ".";
      justify-content: center;
      justify-items: center;
      align-content: center;
      align-items: center;

      width: 100%;
      height: 100%;

      background-color: rgb(255, 246, 232);

      p {
        grid-area: title;
        color: rgb(103, 192, 225);
        font-size: 3vw;
      }

      label {
        grid-area: icon;
        width: 15vw;
        height: 15vw;
        background-position: 50% 50%;
        background-size: contain;
        background-color : white;
        background-repeat: no-repeat;
        cursor: pointer;

        transition: .2s transform ease-in-out;

        &:hover {
          transform: scale(1.05);
        }

        &:after {
          position: absolute;
          display: inline-block;
          content: '';
          width: 3vw;
          height: 3vw;
          background-image: url("../assets/mouse.svg");
          background-position: 50% 50%;
          background-size: contain;

          margin-left: 7.5vw;
          margin-top: 15vw;
          animation: mouse .5s ease-in-out infinite alternate;
        }
      }
    }

    .sponsor_content {
      grid-area: right;

      display: grid;
      grid-template-rows: 5fr 1fr;
      grid-template-areas: "content"
        "ncku";
      justify-content: center;
      justify-items: center;
      align-content: center;
      align-items: center;

      width: 100%;
      height: 100%;

      p {
        grid-area: ncku;
        color: rgb(103, 192, 225);
        font-size: 3.6vw;
      }

      article {
        padding: 6vh 3vw 0 0;
        text-align: left;
        line-height: 3.6vh;
        grid-area: content;

        width: 80%;
      }
    }
  }
</style>
