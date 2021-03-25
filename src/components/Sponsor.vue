<template lang="pug">
  div(class="sponsor_page")
    //div(class="sponsor_title")
    div(class="live_top_bar_pc")
      div(class="live_top_bar_layout" @click="scroll()")
        router-link(tag="label" class="live_exit_button" to="/")
        div(class="live_top_bar_item")
          router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]" v-if="pc") {{text}}
            div(id="bottom" v-if="index===5")
          label(@click="openTab('https://docs.google.com/forms/d/e/1FAIpQLSfx69xLr9XCqz6y8OEn4d8n6gc4qw3KzOn8FHb7Dm94pGwwmg/viewform'); list = false;" v-if="pc") 我要報名
    div(class="live_top_bar_mobile")
      div(class="live_mobile_title" @click="list = false")
      router-link(tag="div" class="live_mobile_exit_button" to="/")
      div(class="live_mobile_list" @click="list = !list")
    div(class="live_mobile_list_area" v-show="list")
      router-link(tag="label" v-for="(text, index) of menuText" v-bind:key="text" v-bind:to="'/' + urlText[index]") {{text}}
      label(@click="openTab('https://docs.google.com/forms/d/e/1FAIpQLSfx69xLr9XCqz6y8OEn4d8n6gc4qw3KzOn8FHb7Dm94pGwwmg/viewform'); list = false;" v-if="!pc") 我要報名
    button(v-show="mode === 1" class="sponsor_return_button" v-on:click="mode = 0; currentIndex = -1;")
    div(class="sponsor_layout_1" v-show="mode === 0" @click="list = false")
      section(class="sponsor_list")
        label(v-for="(iter, index) of sponsorLogo" v-bind:key="iter.name" v-bind:data-name="iter.name" v-bind:href="`${iter.link}`" target="_blank" v-bind:style="{'background-image': 'url(' + sponsorLogo[index].img + ')'}" v-on:click="currentIndex = index; mode = 1;")
        div(class="sponsor_list_empty")
    div(class="sponsor_layout_2" v-if="mode === 1" @click="list = false")
      section(class="sponsor_info")
        p {{sponsor[currentIndex].name}}
        label(v-bind:style="{'background-image': 'url(' + sponsorLogo[currentIndex].img + ')'}" v-on:click="openSponsorTab(currentIndex)")
      section(class="sponsor_content")
        article(class="sponsor_content_article" v-html="sponsor[currentIndex].content")
        article(class="sponsor_content_info" v-html="sponsor[currentIndex].info")
        label(class="sponsor_logo_small")
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
      currentIndex: -1,
      mode: 0,
      sponsorLogo: [
        {
          name: '馥貴春',
          img: 'https://i.imgur.com/a2u62ZH.jpg'
        },
        {
          name: '儕陞生化',
          img: 'https://i.imgur.com/nmO1E7M.png'
        },
        {
          name: '萬鼎工程',
          img: 'https://i.imgur.com/cTmWDli.png'
        },
        {
          name: '哈努曼',
          img: 'https://i.imgur.com/Nja12Ae.jpg'
        },
        {
          name: '成大會館',
          img: 'https://i.imgur.com/IrFzqYk.jpg'
        },
        {
          name: '川益科技',
          img: 'https://i.imgur.com/XDztEZK.jpg'
        }
      ],
      sponsor: [
        {
          name: '馥貴春',
          link: 'http://www.afuque.com/',
          content: `
            <p align="center">
            <p style="padding: 10px 0; font-size: 18px; line-height: 25px; font-weight: bold; color: rgb(46,183,245); letter-spacing: 2px;">
            藏著滿滿孝心的人氣馥貴春重乳酪蛋糕。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 台南的人氣美食馥貴春重乳酪蛋糕，除了美味背後還藏著女兒對父母的孝心。馥貴春創辦人葉小姐說，馥貴春從開始就困難重重，父母一直是她的後盾，支持馥貴春走過五個年頭，也讓越來越多人知道馥貴春重乳酪蛋糕的美味。
            </p>
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 一塊有著金黃的自然色澤，從外觀就看得出不凡的重乳酪蛋糕，品嚐的瞬間更是能感受到它綿密的口感。甚至是目前網路知名的人氣台南伴手禮、隱藏版美食，它就是馥貴春重乳酪蛋糕。而製作出如此精緻重乳酪蛋糕的，是一位七年級女生憑著毅力與堅持，更是出自對爸爸的孝心，自行創業並打響「馥貴春重乳酪蛋糕」的招牌。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 淬鍊兩年，爭取到晶華酒店實習機會馥貴春重乳酪蛋糕的創辦人──在台南長大的葉小姐，因為從小就喜歡烘培與烹飪，靠著努力進入知名的國立高雄餐旅大學就讀，經過不斷認真學習，培養相當程度的廚藝。在大三時、兩年的校內學習後，爭取到國際五星級飯店台北晶華酒店101館外餐廳的實習機會，並被派到點心檯，遇到了生命中最大的貴人。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 小小年紀，便能跟在烘焙界的翹楚「黃師父」身邊學習，是七年級生葉小姐想都沒想到的幸運事，黃師父是全台獲獎無數的點心名廚，烘焙經驗累積數十年，是業界相當權威的蛋糕點心代表高手，而黃師傅所製作的蛋糕點心中，「重乳酪蛋糕」是最令大家嘖嘖稱奇、難以忘懷的名品，只要有幸吃到一口，便能體會「舌尖上的幸福」真正的含意。許多客人到台北晶華酒店101館外餐廳用餐，甚至只為吃到黃師傅所製作的重乳酪蛋糕，可想而知，有多美味珍貴。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 囡仔，以後自己要好好加油跟在黃師傅身邊學習的日子，非常地充實與戰戰兢兢，黃師傅對葉小姐的教學相當嚴厲，卻也真心疼愛她。但天下無不散的筵席，某天，黃師傅突然語重心長地對她說：「囡仔，師父我老了、也累了，要從飯店退休。以後妳自己要好 好地加油。」縱使不捨至極，眼淚欲出，葉小姐也明白黃師傅終究已做好退休打算。想到自此無法天天看到師傅、品嚐到師傅親手製作的蛋糕，就讓葉小姐萬分感傷，而就在黃師傅退休的前一天，在葉小姐的央求下，因為她總是勤奮而認真的態度，黃師傅答應教她這獨門的「重乳酪蛋糕」，而師傅親傳手藝的重乳酪蛋糕，是葉小姐從黃師傅身上學到的最後一個蛋糕，卻也是改變她人生最重要的一個 蛋糕。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 台北實習一年後，葉小姐回到高雄繼續就讀國立高雄餐旅大學。畢業前夕，爸爸正好面臨牙齒整修期，根本無法嚼食硬性食物，看著喜歡吃甜食的父親無法盡情享用，為了體貼父親，母親常常購買好入口的蛋糕，好讓父親滿足口慾，但看在葉小姐眼裡，這些蛋糕都不夠營養美味。為了讓父親在享受甜點美食的同時，也能更加健康，葉小姐決定製作跟黃師傅學習的重乳酪蛋糕，讓父親當作日常餐點，卻沒意料到，親手製作、冷凍過後的重乳酪蛋糕，不僅受到父母親的肯定，連前來家裡拜訪的客人都驚為天人，甚至在同年母親節前夕，特地致電來委託訂購重乳酪蛋糕。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 重乳酪蛋糕的好滋味，就這樣傳了出去，沒預計未來會創業的葉小姐，在當次的母親節，居然接到10盒重乳酪蛋糕的訂單。因為沒設備、沒器具的情況下，葉小姐求助學校老師幫忙，利用學校設備製作蛋糕，卻偏偏受到某位老師的刁難……，總是默默支持她的父親，在得知這樣的狀況發生後，安慰並鼓勵她：「沒關係，別難過。不如我們買一台烤箱，自己在家做，就當做是妳畢業後的創業」。
            </p>
            <p><img src="https://i.imgur.com/sJZNB1D.jpg") style="border:2px white solid;padding:5px; min-width: 250px; min-height: 250px; height:35vw;"></p>
            <p style="padding: 10px 0; font-size: 18px; line-height: 25px; font-weight: bold; color: rgb(46,183,245); letter-spacing: 2px;">
            藏著滿滿孝心的人氣馥貴春重乳酪蛋糕。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 在全家人的肯定與支持下，葉小姐在畢業後開啟了創業之路，為了專心製作重乳酪蛋糕，目前先推出的商品，就是傳承於黃師傅手藝的美味重乳酪蛋糕，然後自己再從中研發、改良，並一手包辦馥貴春蛋糕外盒包裝、與蛋糕上的手工拉花設計。身為七年級生，獨自踏上創業之路非常地艱辛，常遇到挫折和失敗。但葉小姐擁有七年級少見的韌性和堅持，一開始因為知名度還沒打開，資金有周轉不靈的情形，但她沒有因此被打敗，仍是每天堅持用最好的原料做出重乳酪蛋糕，總是有多少資金就做多少重乳酪蛋糕，甚至到處請人試吃，讓更多人發現馥貴春的美味，漸漸地，馥貴春的知名度也從台南向台灣各地飄香出去。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 爸媽總是看著葉小姐做蛋糕粗糙的手，心疼地掉淚。應該是擦上漂亮指甲油或和朋友四處遊玩的年紀，但葉文玉為了堅持夢想即使是炎熱的天氣，仍舊在悶熱的中央廚房努力耕耘著。看著女兒把馥貴春做得有聲有色，父母也漸漸放心，心中常掛心的孩子，已經是一個獨立而且擁有自己品牌的女兒。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 葉小姐說，這些辛苦和父母小時候養育我們的辛苦比，根本不算什麼，她認為將馥貴春重乳酪蛋糕做好，不只是對馥貴春這個品牌的責任，更是她想好好孝順父母的禮物。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 「只要妳有心，我相信妳就能克服這些難題。」父親的話，是葉小姐不放棄的動 力，這原先單純為了父親而製作的蛋糕，目前在台南已有兩家人氣實體店面。無數網友、台南居民都肯定的美味馥貴春，正在一步步茁壯，以堅持師承台北晶華酒店──黃師傅手藝的蛋糕好滋味，讓大家都能品嚐到綿密、入口幸福感湧現的馥貴春重乳酪蛋糕，終於，在今日闖出了自己的一片天。
            </p>
            `,
          info: `
            <p align="center">
            <p style="color:rgb(42,186,243); letter-spacing: 2px; font-weight: bold; font-size: 2.5vh; line-height: 3.8vh;">馥貴春</p>
            <p>官網 : <a href="http://www.res.com.tw/"> http://www.res.com.tw/</a></p>
            <p>地址 : 台南市永康區永大五路151號</p>
            <p>營業時間 : 10:00-21:00</p>
            </p>`
        },
        {
          name: '儕陞生化',
          link: 'https://www.charsire.com.tw/',
          content: `
            <p align="center">
            <p style="padding: 10px 0; font-size: 18px; line-height: 25px; font-weight: bold; color: rgb(46,183,245); letter-spacing: 2px;">
            儕陞生化以CORE LGF 黑科技於2021亞太美業精品獎中榮獲「產業標竿獎」殊榮。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 在國內以研究阿茲海默症、血管性失智藥品以及用於潰瘍傷口用藥逐漸開啟企業品牌知名度的儕陞生化技術公司，110年新春開紅盤，除於2021亞太美業精品獎中獲頒「產業標竿獎」外，更獲得台灣衛福部認證用於創傷與燒傷的二類醫材上市許可，該醫材也是台灣第一支植物性二類醫材；由於研產銷在地化，預計，將能快速滿足國內各大醫療院所於外用敷料的高度使用。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 該公司總經理翁志宜表示，儕陞生化是一家以植物新藥作為醫藥研發的生技公司，所開發的小分子萃取技術，係從天然植物中取得所需物質且要求極低負作用下所新創；現階段已有阿茲海默症及血管型失智症新藥已完成美國FDA 核准之二期臨床試驗，另糖尿病潰瘍傷口藥品已完成國內二期臨床解盲，整體研發進度如質如實的進行中。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 翁志宜指出，正由於二十年來的堅持與不懈怠，研發成果得已從實驗室走向應用市場；此次從素有生技美容界奧斯卡美譽的2021亞太美業精品獎中獲「產業標竿獎」，正是儕陞生化的重要里程；該大獎主要是嚴選在業界具代表性的廠商，以企業業績成長值、通路布局全球化及產業未來發展性等三大方向作為主要評選依據。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 翁志宜坦言，要站穩生物醫藥市場，必須要有專精的生物技術，而儕陞生化所獨創的植物萃取黑科技，至今已超過二十年，該生物技術能精確提煉有效成分CORE LGF，經臨床實驗交叉證明，CORE LGF能夠激活PDGF。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 而PDGF在許多科學研究中發現，是由特定細胞所分泌的生長因子，在微血管維護上扮演重要角色，具有刺激特定細胞群分裂增殖的能力，可強化微循環系統，同時能協助膠原蛋白不斷機轉，是研究如何保養肌膚輕彈的關鍵因素。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 事實上，儕陞團隊運用黑科技，在新藥開發、醫療器材外用敷料、醫美保養品及外用保健產品等已有顯著進展，如失智及糖尿病潰瘍傷口與放療皮膚損傷治療，分別完成美國FDA二期臨床試驗及台灣衛福部一期臨床臨床試驗外，近日更獲得台灣衛福部通過二類醫材認證，這是台灣目前第一支植物性二類醫材，適用於創傷與燒傷，由於係本土廠商自行成功研發製造，除極低負作用，並可在地即時供應，將有效舒緩國內外用敷料的高度使用；另，同時在越南也通過二類醫材認證，對拓展東南亞市場，相信可以更加順利。
            </p>
            `,
          info: `
            <p align="center">
            <p style="color:rgb(42,186,243); letter-spacing: 2px; font-weight: bold; font-size: 2.5vh; line-height: 3.8vh;">儕陞生化</p>
            <p>官網 : <a href="https://www.charsire.com.tw/"> https://www.charsire.com.tw/</a></p>
            <p>地址 : 744台南市新市區南科二路13號</p>
            </p>`
        },
        {
          name: '萬鼎工程',
          link: 'http://www.rei.ctci.com/',
          content: `
            <p align="center">
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 萬鼎工程服務股份有限公司為CTCI中鼎集團成員，成立於1984年。萬鼎的工程經驗豐富，深度了解業主需求及當地環境特色，為土地營造新願景。 萬鼎工程是工程統包之領導廠商，提供公共工程統包解決方案，尤其在土木建築和交通工程領域累積豐富的工程經驗和技術。服務內容涵蓋工程規劃、設計、興建、測試、驗證，並專精冰凍工法以及智能頂進工法。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 萬鼎工程積極經營建築、交通、大地、潛盾、水利、環境、電機、土地開發等市場，穩居台灣統包工程產業領先地位。
            </p>
            </p>`,
          info: `
            <p align="center">
            <p style="color:rgb(42,186,243); letter-spacing: 2px; font-weight: bold; font-size: 2.5vh; line-height: 3.8vh;">萬鼎工程</p>
            <p>官網 : <a href="http://www.rei.ctci.com/"> http://www.rei.ctci.com/</a></p>
            <p>台北辦公室-地址: 台北市南港路三段48號4樓</p>
            <p>高雄辦公室-地址: 高雄市橋頭區成功路163號</p>
            </p>`
        },
        {
          name: '哈努曼',
          link: 'https://www.facebook.com/HANUMAN20181203/?ref=page_internal',
          content: `
            <p align="center">
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 哈努曼泰茶裡，販售的不只是飲品，更有滿滿的大小故事等您來挖掘，泰奶風潮得掘起，能見度大展開，想喝泰奶不在是件難事，但您喝到的是泰奶，還是把香氣不到位，不甜合理化的台泰奶呢？
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 一樣的茶葉但調理後的味道大大不同，就如一樣學習考試測驗，用心學習懂的精髓的成績當然好，有樣學樣、只懂皮毛的成績當然差強人意，哈努曼泰茶也就是單純的想把家鄉的味道，好好的留存，給各位好朋友們品嚐，不以最大的獲利率為出發點，而是以－最完整的家鄉味與故事來跟各位分享傳遞！
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            坊間泰奶能給您的，哈努曼絕對能給您的感受更多，就是要讓您深刻感受～🙏🙏🙏
            </p>
            <pre padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            </pre>
            <div style="background-image: url('https://i.imgur.com/035EwTG.png'); background-position: right 10% center; background-repeat: no-repeat; background-size: 50%;">
            <pre style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
……………………………………………………………
台南地區
首間泰式飲品專賣店
道地泰式手工茶飲
純正泰國人經營
隱身在小巷弄裡
堅守著純正的味道
打造純正泰國的氛圍感受
一秒瞬間融身泰國
🇹🇭🇹🇭🇹🇭🇹🇭🇹🇭🇹🇭🇹🇭🇹🇭🇹🇭🇹🇭🇹🇭🇹🇭🇹🇭 
台南喝泰式飲品
首選當然就是哈努曼泰茶
沒喝到哈努曼泰茶前
別對泰式奶茶輕易妥協
不跟隨熱鬧 只專精門道
你終究要喝泰奶的
那為什麼不一開始就來哈努曼泰？
想喝杯泰奶不需要冒生命危險
來趟哈努曼就能滿足您想喝泰奶的慾望
……………………………………………………………
            </pre>
            </div>
            </p>`,
          info: `
            <p align="center">
            <p style="color:rgb(42,186,243); letter-spacing: 2px; font-weight: bold; font-size: 2.5vh; line-height: 3.8vh;">哈努曼</p>
            <p>粉絲專頁 : <a href="https://www.facebook.com/HANUMAN20181203/?ref=page_internal"> https://www.facebook.com/HANUMAN20181203/?ref=page_internal</a></p>
            <p>地址 : 701 臺南市東區崇善二街12號</p>
            <p>營業時間：10:00-21:30（每週二固定公休）</p>
            </p>`
        },
        {
          name: '成大會館',
          link: 'http://www.zendasuites.com.tw/zh-tw',
          content: `
            <p align="center">
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 成大會館位於綠意盎然的成大校區內，客房內皆可觀看美麗的校園美景，每年花季，燦爛美麗的木棉花、黃花風鈴木、阿勃勒、鳳凰花等依時序綻放，美不勝收是大城市裡十分難得的住宿地點。其外觀承襲成大校舍原有的橘紅色系建築，與校園的景致融為一體，搭配上仿巴洛克式的樑柱與白色拱門，散發著台南古都的歷史韻味，常吸引遊客駐足留影。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 會館客房擁有12坪的寬敞空間，是小家庭及三五好友出遊的最佳選擇。兩張KING SIZE的大床搭配高級的羽毛絨被套組，讓您一覺好眠。大片的落地窗，放眼望去就是成功大學的美麗校景，坐在窗邊的沙發上，或看書、或喝杯咖啡，就能體會度假的悠閒。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 早餐區聘請飯店主廚，製作美味的提供中西式自助早餐，供餐的空間雖不大，內容卻十分豐富；不但可吃到港式的燒賣，美式的大亨堡，日式的喬麥麵與中式的清粥小菜，而府城在地的傳統小吃，如米糕、鹹粥、碗粿、肉躁飯等，更是每天在此輪番上陣，不但讓您吃得飽足、營養、有精神，更透過美食讓您體驗一遭府城的常民美食文化。
            </p>
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 步出會館即進入校區，校園內的列級古蹟及歷史文物相當多，如成大博物館即以日治時代的行政大樓改建，內部設有校史室；光復校園的大成館則是國定的百年古蹟，面對著成大著名的景點－榕園；而楊柳婆娑的成功湖，古色古香的歷史博物館，及歷經烽火存留下來的小西門遺址，都是您不可錯過的悠遊景點。
            </p>
            </p>`,
          info: `
            <p align="center">
            <p style="color:rgb(42,186,243); letter-spacing: 2px; font-weight: bold; font-size: 2.5vh; line-height: 3.8vh;">成大會館</p>
            <p>官網 : <a href="http://www.zendasuites.com.tw/zh-tw"> http://www.zendasuites.com.tw/zh-tw</a></p>
            <p>地址 : 701台南市東區大學路2號</p>
            <p>電話 : 06 275 8999</p>
            </p>`
        },
        {
          name: '川益科技',
          link: 'https://www.kingslide.com/',
          content: `
            <p align="center">
            <p style="padding: 5px 0; font-size: 15px; line-height: 22px; color: rgb(120, 120, 120); letter-spacing: 2px;">
            &nbsp &nbsp &nbsp &nbsp 於2006年成立在高雄科學園區內的川益科技，以創造消費需求而創立， 結合ID設計元素及專業研發人才研究與開發頂尖的先進製程技術與材料應用並投入電子零組件與消費性電子產品產業， 運用多種嶄新技術研發製造出精度高、觸感佳、質感優的金屬配件，成功引領3C產品朝向多樣化與個性化的方向發展。 卓越的研發技術與優良品質使King Slide品牌不僅在雲端產業與高階廚具產業獲得業界翹楚之指定品牌，在邁入消費性電子產品的領域發展之際， King Slide功能性機構產品已朝向全方位應用端發展，現已成為各高科技大廠及各行業翹楚指名的長期合作伙伴。
            </p>
            </p>`,
          info: `
            <p align="center">
            <p style="color:rgb(42,186,243); letter-spacing: 2px; font-weight: bold; font-size: 2.5vh; line-height: 3.8vh;">川益科技</p>
            <p>官網 : <a href="https://www.kingslide.com/"> https://www.kingslide.com/</a></p>
            <p>地址 : 82151台灣高雄市路竹區路科九路6號</p>
            <p>電話 : +886-7-976-1688</p>
            </p>`
        },
        {
          name: '',
          link: '',
          contetn: '',
          info: ''
        }
      ]
    }
  },
  mounted: function () {
    this.pc = this.isPC()
    this.setBarHeight()
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
    .sponsor_page {
      display: flex;
      align-items: center;
      justify-content: center;
      min-width: 1000px;
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
      .live_mobile_title {
        z-index: 100;
        grid-area: title;
        width: 60vw;
        background-image: url('../assets//sponsor/title.svg');
        background-repeat: no-repeat;
        background-size: 75% 75%;
        background-position: center center;
      }
      .live_mobile_list {
        z-index: 100;
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
    .live_mobile_list_area {
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

    .sponsor_title {
      position: absolute;
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 20;
      top: 3vh;

      width: 45vw;
      height: 20vw;
      background-image: url("../assets//sponsor/title.svg");
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background-position: center top 10%;
    }

    .sponsor_return_button {
      position: absolute;
      right: 7vw;
      top: 10vh;

      width: 7.5vw;
      height: 7.5vw;
      background-color: transparent;
      background-image: url("../assets//return.svg");
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
      grid-template-rows: 1fr 3fr 20vh;
      grid-template-areas: "top"
        "bottom"
        ".";
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
      grid-template-rows: 1fr 5fr;
      grid-template-areas: "top" "bottom";
      justify-content: center;
      align-content: center;
      align-items: flex-start;

      background-color: rgb(255, 246, 232);
      width: 100vw;
      height: 92vh;
      // top: 8%;
      overflow-y: scroll;
    }

    .sponsor_list {
      grid-area: bottom;

      display: grid;
      grid-template-rows: repeat(10, 60vw);
      grid-gap: 7vh;
      justify-content: center;

      width: 100vw;
      height: 75vh;
      overflow-y: scroll;

      label {
        width: 60vw;
        height: 60vw;
        border-radius: 30px;
        border: 10px solid transparent;
        box-shadow: 1px 1px 2px 1px rgba(50, 50, 50, 0.3);
        background-color: white;
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
            border-radius: 30px;
            background-color: rgba(0, 0, 0, 0.66);
            z-index: 20;
          }
          &:after {
            position: absolute;
            content: attr(data-name);
            color: white;
            margin-top: 10vw;
            margin-left: -30vw;
            width: 60vw;
            z-index: 21;
          }
        }
      }
      .sponsor_list_empty {
        height: 15vh;
        width: 100%;
      }
    }
    .sponsor_content {
      grid-area: bottom;

      display: grid;
      grid-template-rows: 50vh 1fr 8vh;
      grid-template-areas: "content" "info" ".";
      justify-content: center;
      justify-items: center;
      align-content: center;
      align-items: flex-start;

      width: 96vw;
      height: 80vh;

      .sponsor_content_article {
        text-align: left;
        grid-area: content;
        line-height:3vh;

        width: 92vw;
        height: 50vh;
        overflow-y: scroll;
        overflow-x: hidden;
      }

      .sponsor_content_info {
        text-align: left;
        grid-area: info;
        line-height:3vh;
        padding: 1vh 0 0 0;
        width: 96%;
        height: 8vh;
      }
    }

  }

  /*
    computer layout css
  */
  @media only screen and (min-width: 600px) {    @font-face {
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
      background-image: url("../assets//sponsor/title.svg");
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
      background-image: url("../assets//return.svg");
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
      grid-template-rows: 2.4fr 1.8fr 0.8fr;
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
      grid-template-columns: repeat(6, 10vw);
      grid-template-rows: repeat(3, 10vw);
      grid-gap: 3vw;
      justify-content: center;
      align-items: center;

      width: 100%;
      label {
        width: 10vw;
        height: 10vw;
        border: 5px solid transparent;
        border-radius: 20px;
        background-position: 50% 50%;
        background-size: contain;
        background-color : white;
        background-repeat: no-repeat;

        transition: .2s transform ease-in-out;
        cursor: pointer;
        &:hover {
          &:before {
            position:absolute;
            display: block;
            text-align: center;
            content: '';
            width: 10vw;
            height: 10vw;
            border-radius: 20px;
            background-color: rgba(0, 0, 0, 0.66);
            z-index: 20;
          }
          &:after {
            position:absolute;
            content: attr(data-name);
            color: white;
            text-align: center;
            font-size: 20px;
            margin-top: 3vh;
            margin-left: -5vw;
            width: 10vw;
            height: 10vw;
            z-index: 21;
          }
        }
      }
    }

    .sponsor_info {
      grid-area: left;
      display: grid;
      grid-template-rows: 0.5fr 1fr 2fr 1fr;
      grid-template-areas: "."
        "title"
        "icon"
        ".";
      justify-content: center;
      justify-items: center;
      align-content: center;
      align-items: center;
      // transform: skewX(3deg);

      position: absolute;
      left: -2vw;
      width: 100%;
      height: 100%;
      background-color: rgb(255, 246, 232);
      box-shadow: 0 0 3px 2px rgba(100, 100, 100, 0.3);

      p {
        grid-area: title;
        color: rgb(48,182,245);
        font-size: 40px;
        letter-spacing: 4px;
        // transform: skewX(-3deg);
      }

      label {
        grid-area: icon;
        width: 18vw;
        height: 18vw;
        background-position: center center;
        background-size: contain;
        background-color : white;
        background-repeat: no-repeat;
        border-radius: 30px;
        box-shadow: 1px 1px 2px 1px rgba(55, 55, 55, 0.3);
        border: 15px solid transparent;
        transform: skewX(-3deg);

        cursor: pointer;
        transition: .2s transform ease-in-out;
        &:hover {
          // transform: scale(1.05) skewX(-3deg);
          transform: scale(1.05);
        }
        &:after {
          position: absolute;
          display: inline-block;
          content: '';
          width: 3vw;
          height: 3vw;
          background-image: url("../assets//mouse.svg");
          background-position: 50% 50%;
          background-size: contain;

          margin-left: 10vw;
          margin-top: 20vw;
          animation: mouse .5s ease-in-out infinite alternate;
        }
      }
    }

    .sponsor_content {
      grid-area: right;

      display: grid;
      grid-template-rows: 4fr 1.2fr 0.5fr 0.2fr;
      grid-template-areas: "content" "info" "logo" ".";
      justify-content: flex-start;
      justify-items: flex-start;
      align-content: flex-start;
      align-items: flex-start;
      padding: 8vh 2vw 3vh 4vw;

      width: 100%;
      height: 90%;

      .sponsor_content_article {
        padding: 0 10px 0 5px;
        text-align: left;
        line-height: 3.6vh;
        grid-area: content;
        width: 85%;
        height: 52vh;
        overflow-y: scroll;
        &::-webkit-scrollbar {
          width: 0.6vw;
          border-radius: 0.5vw;
        }
        &::-webkit-scrollbar-thumb {
          background: rgb(103, 192, 225);
          border-radius: 0.5vw;
        }
      }
      .sponsor_content_info {
        grid-area: info;
        padding: 12px 0 0 5px;
        border-top: 3px solid rgb(254,241,217);
        border-radius: 1px;
        text-align: left;
        width: 65%;
        color: rgb(60, 60, 60);
        font-size: 15px;
        line-height: 25px;
      }
      .sponsor_logo_small {
        grid-area: logo;
        margin: 0 0 0 38vw;
        width: 15vw;
        height: 6vh;
        background-image: url("../assets//logoSmall.svg");
        background-position: left center;
        background-size: contain;
        background-repeat: no-repeat;
      }
    }
  }
</style>
