## 比賽策略

---

重點不是在你學了多少進階資料結構或演算法，而是在你要去練習怎麼觀察題目

子題通常都很有引導性，要練習想子題，在利用子題想正確的滿分解

先想 chain, tree 的 case

先想何為無解的情況

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>:fontawesome-solid-user:</font> cthbst</font>

---

- 考前
	- 平時
		- 學習新技術
		- 複習、練習
		- virtual
	- 接近
		- 不要碰新東西
		- 複習模板
- 喇分
	- 小測資
	- 提示滿分解
	- 時間策略
	- **不要有 0 分的題目**
- 時間
	- 讀完所有題目
	- 不用從頭寫到尾
	- 了解自己能力與目標
	- 不要鬼打牆
	- 子題與滿分解
- 心態
	- 適時換題
	- 不要管別人在幹嘛
	- \#相信
- 其他
	- 讀題時標記難度想法
	- 吃東西尿尿
	- 最後一分鐘都可以翻盤

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>:fontawesome-solid-user:</font> Ccucumber12</font>

---

正賽

1. 花約半小時打模板 + 看完**所有**題目，後面題目記得看部分分，然後稍微想一下第一直覺的想法，並把水題(馬上有好實作解的題目)挑出來<br>
2. 先把水題寫完，剩下題目每次留三題最可做 or 部分分少的題目輪流想，一題想20分鐘，時間到就要跳，不能一直停在某題<br>
3. 前三小時思考盡量以滿分解為主，最後兩小時以難題的部分分為主

比賽可能的狀況

滿分解WA

1. 去趟廁所，花5分鐘先看過一遍code，看看有沒有刻爆<br>
2. 重新想自己的想法有沒有爛，構構看反例<br>
3. 寫對拍<br>
4. 除蟲時間最多20分鐘，一到就要跳

TLE

1. 花5分鐘先看過一遍code，看看有沒有刻爆<br>
2. 本機生大測資，看是想法爛還是卡常<br>
3. 卡常：線段樹 -> BIT or 靜置、vector -> array

RE / MLE

1. 戳到陣列外面<br>
2. assert

judge爛了

- 不確定的題目先寫對拍簡單驗一下

其他事項

1. 遇到大實作 or 大資節題先想清楚脈絡再寫<br>
2. 記得上廁所<br>
3. 檔名存好，之後對拍較方便<br>
4. 吃飯deadline：12:30

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>:fontawesome-solid-user:</font> abc864197532</font>

---



1. 實作：<br>
   Q. 雖然我的思考也挺爛，但實作的問題還是最大。我都一直在挑戰難題和思考題，雖然很有趣，但變成我的題目量很少，導致我很多C++內部的東西其實都沒搞懂，都只是把它搞到會過而已，沒有好好理解。

2. 邊寫邊想：<br>
   在寫很多CF梗題的時候其實就該意識到這一點，我都一直想寫出東西來，沒有好好的思考題目，找到最適合和正確的解法。尤其是在我很急的時候，這一個問題就被無限地放大了(就是全國賽)。

3. 比賽策略 :<br>
   這個其實應該是最輕微的，因為說實話如果你實力不好(我)，再怎麼規劃比賽策略都是屁，因為你根本達不到你預想的策略(例如：想說前三個小時拼AC解但根本拼不到)。但初選的時候還是要規劃完善的策略，避免燒雞，所以還是很重要

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>:fontawesome-solid-user:</font> guagua</font>

---

模考是一場**五小時四題**的比賽
要在一場長時間的比賽中穩定發揮
比賽策略就是關鍵

實用技巧

- 暫時休息
    - 喝水、吃東西、上廁所
- 清空腦袋
    - 不要一直卡在同一個想法！
    - 同上
- 在題本封面寫下所有子題的分數，並把會做與 AC 的子題作記號
- 卡題停損點
- 子題很有用

比賽的目標

- 比賽的目標只有一個：maximize 你的分數
- 所以比賽中你不應該想
    - 別人一定都會這個 / 別人一定都不會這個
    - 我這樣可以上二階 / 當國手嗎
    - 如果我打爛要怎麼辦

時間分配

- 初期：如何開題？做題順序？
- 中期：小心卡題
- 後期：記得喇分、適度放棄

Debug

- `assert`（還可以自己寫 WA 版的 assert）
- 輸出訊息
    - 印陣列、pair
- 暴力解對拍
    - 小測資很好用

---

模考前一天 ~ 模考前

- 完全耍廢
    - 看動畫
        - 把整週的新番留下來看
    - 看小說
        - 安利美特
    - 拼圖
- 提醒自己的話
- 每天都要發心得

賽中

- 前 30 分鐘
    - 讀題、每題都有個初步想法
    - 有時開場會因為緊張想上廁所
- 先從有把握的題目開始做
    - 先拿一些分數提升自信
- 只要實作小複雜就先寫小子題
    - 用 judge debug 愛用者 (?)
- 通常會在前 2 個小時把剛開場想到的東西都寫掉
    - 含所有不難寫的喇分
- 接下來大部分時間都是各題交替著想
- 接近賽末的時候（約 1 個小時）如果沒有在寫東西，把所有題目再掃一次，有時會忽然想到



- 每個小時吃一次東西
    - 順便當成一種提醒自己時間的方式
- 一直上廁所、喝水
    - 大部分的想法都是在廁所裡想的
    - 一場比賽上 6~7 次廁所
    - ~~但如果每個人都這樣男廁會塞爆~~
- 一定要乖乖 (?)
    - 不能重複使用
- 摸摸吉祥物
- 我高二開始就不會在賽中玩踩地雷了，不要再抹黑我了

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>:fontawesome-solid-user:</font> wiwiho</font>

---

模考前一天：

- 耍廢，but how？

我曾經在模考前做過的事情：

- 看動漫
- 讀漫畫（實體）
- 看紀錄片
- 畫素描
- 玩三個小時的 osu!
- 玩合成器（音樂）
- 拼拼圖

還有早睡 (大概 10:30 前) 很重要

模考當天：

- 早餐吃正常的食物，不要拿日式燒雞
- 記得準備自己的點心，最好是已經吃過的食物
- 在考試開始前放空

考試時：

- 前一小時看完所有題目，並且確認可以做的子題
- 一小時之後先去拿可以拿的分數
- 拿到之後根據比賽時間有兩種可能
	- 1.5~2 小時，找一個可以拿到很多分數的題目花時間想
	- 2.5~3 小時，直接看每個子題有什麼可以做

(這是我高三的策略，程度不同可以用不同策略)

其他注意事項：

- 喝水（我一場模考可能會喝到 1500 ml）+上廁所
- 一般來講是在要開始寫某個東西前或是要確認某個想法時
- 吃東西：大約 2hr, 3hr, 4hr 各一次。3hr 會吃比較多
- 摸摸吉祥物

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>:fontawesome-solid-user:</font> 8e7</font>

---

時間分配 :

前 30 分鐘 ( 2:00 - 2:30 )

1. 讀完所有題目
2. 寫簽到題
3. 排序題目難度

接下來一小時 ( 2:30 - 3:30 )

1. 從簡單的題目開始想
2. 每題花 10 分鐘想
3. 如果有滿分解，就想一下實作細節，開始寫
4. 如果只有部分分，就先想下一題
5. 優先做有滿分解的，如果兩題都只有部分分就寫比較簡單的
6. 想清楚再寫
7. 如果寫到一半發現想錯了，就先做另一題
8. 寫 + debug 如果超過 25 分鐘也先做另一題
9. 後 1.5 小時 ( 3:30 - 5:00 )
10. 看最難的兩題
11. 先各花 10 分鐘檢視一下能拿的部分分有哪些
12. 每題花 20 分鐘拿部分分，時間到就跳題
13. 除非很有把握，不然先以一定拿得到的分數為主

其他事項 :

- 如果在 “接下來一小時” 覺得難度有點不對勁，就直接進入撈分環節

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>:fontawesome-solid-user:</font> Paul-Liao</font>

---

- stuff you should look for

- int overflow, array bounds

- special cases (n=1?)

- do smth instead of nothing and stay organized

- WRITE STUFF DOWN

- DON'T GET STUCK ON ONE APPROACH

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>:fontawesome-solid-user:</font> Benq</font>

## 經驗分享

- [IOIC 2023 經驗分享](https://hackmd.io/@baluteshih/ioicamp-day1#/)
- [TOI 2023 一階選訓營經驗分享](https://hackmd.io/@wiwiho/HJoRJ10Rj)
- [TOI 2023 一階選訓營題型介紹](https://hackmd.io/@wiwiho/BkgXBX4kg2)
- [TOI 2021 一階選訓營經驗分享](https://hackmd.io/@baluteshih/BkYA9D9XO)
- [TOI 2021 比賽心態準備](https://hackmd.io/@ToMmyDong/H10t23gE_?utm_source=preview-mode&utm_medium=rec#/)
- [海牛 嘉義高中](https://cs.cysh.cy.edu.tw/old_version/php_system/acm_statistics/%E8%B3%87%E8%A8%8A%E5%AD%B8%E7%A7%91%E8%83%BD%E5%8A%9B%E7%AB%B6%E8%B3%BD%E8%88%87%E8%B3%87%E8%A8%8A%E5%A5%A7%E6%9E%97%E5%8C%B9%E4%BA%9E%E9%81%B8%E8%A8%93%E7%87%9F%E5%82%B3%E6%89%BF_%E5%90%B3%E5%AE%97%E9%81%94_%E5%BC%B5%E5%87%B1%E5%82%91.docx)
