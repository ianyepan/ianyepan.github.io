---
title: Landing a FAANG internship - 2022美國暑期找實習分享
author: Ian Y.E. Pan
date: 2022-01-23 19:49:00 -0500
categories: [General]
tags: [tips, chinese, programming]
---


![FAANG](/images/faang.png){: width="60%"}

# 前言

這篇將著重在 2021 秋天 9-12 月我投履歷+刷題+準備面試+淺談薪資+比較offer 的心路歷程。

這兩三個月來我主要瞄準 2022 Summer US Software Engineer (SWE/SDE) 的實習職缺。另外我也有投一些 quant (quantitative researcher, quantitative trader) 的職位，不過因為市場比較小眾，而且自己在 quant 的部分目前戰績並不理想，所以這次心得還是把篇幅留給我找 SWE 軟體工程師 internship 的經驗。(之後可能會再寫一篇專門聊quant。)

# 背景

-  **2021-現在** Columbia University, Master of Science, Computer Science
-  **2017-2021** 香港中文大學, Bachelor of Science with Honors, Computer Science
-  **2014-2017** 台北建國高級中學

# TL;DR

以下是我碩士第一個學期面試拿到的三家軟體工程師 internship offer:

- **Amazon** Software Development Engineer - Bay Area, CA
- **Tapad** Software Engineer
- **Visa Research**, Machine Learning

如果下學期有別的好消息的話，我會再補一篇下集。

# 刷題準備面試

找軟體工程師的實習我覺得最重要的無非就是刷 LeetCode 的題目。在念本科的時候就有聽過這種說法，我就隨便刷了一百題以為自己很厲害 (而且大部分還是 LeetCode Easy)，沒想到來 Columbia 念書後發現身邊的人平均都有兩三百題，微信群上的內地同學們甚至有人已經四五百題了，我才如醍醐灌頂一樣從夢裡醒來。

刷題起步很痛苦，一題有時要花半個小時到一個小時才能全盤理解。但養成習慣、找到一個節奏感之後其實是一件蠻有成就感的事情。

從開始投履歷到現在我從100題刷到了650題，尤其是我十一月下旬同時準備 Amazon 和 Tapad 的 final round，兩個禮拜刷了兩百多題，算是卯足了勁，從早練到晚。

我主要練習 Medium 的題目，累了就寫個 Easy 當放鬆。 Hard我基本上只刷確定有面試的特定公司的題庫 (i.e. Amazon, Robinhood)，因為出題率相對較低。


![LeetCode](/images/leetcode.png){: width="80%"}
_My LeetCode progress, as of Dec. 2021_

題數累積起來之後，明顯感覺到寫演算法實力的提升，面試實戰發揮起來也順暢許多。刷到五百題左右時，我面試已經不會緊張了，甚至是沒看過的題目我也能靜下心來思考能比照的解題方式。

如果目前沒有刷題方向的朋友們可以參考一位臉書 tech lead 整理出來的黃金七十題，[我也把它整理成一個 GitHub repo](https://github.com/ianyepan/top-70-leetcode-questions)，內含我的解答、思路、和題目連結，可以參考一下。

BQ (behavioral question) 的準備方面我習慣把以前的實習經驗編成情境式的小故事。每個故事我也會有一個長的和一個短的版本。這樣無論時間緊迫與否，我都可以穩定發揮。準備 Amazon 的時候，因為聽說他們家特別看重員工的經驗和價值觀有沒有符合[企業文化](https://www.amazon.jobs/en/principles)，我甚至針對最常考的一些問題打了一份快十頁的個人經驗故事 bullet points，直接當成演講稿在背。依學長姊的建議和我的個人經驗，BQ 非必要不建議即興回答。

另外，我也特別注意在講述自己經驗時，要帶入語氣和用詞的熱情。往往你的 passion 可以感染到面試官，讓他們自然地聽的有興趣一些，對你比較容易留下深刻的印象。

# 面試經驗

這邊總結目前拿到錄取的三家公司，當作經驗分享。

## 1. Visa Research, Machine Learning

![Visa Research](/images/visa_research.png){: width="20%"}

Visa Research Team 的工作職責主要是介在 SWE (software engineer) 跟一群 PhD researcher 中間，幫忙研究拓展部門開發 model 和 prototype，需要對區塊鍊跟 ML 有相關經驗的人。
第一關電話面試考了一道 Medium 題，我聽完立刻找到最佳解，用 hash map + min heap 一次跑完無 bug。


幾天後的第二關面試考官上來直接考了道沒看過的 Hard。我光理解題目就花了十多分鐘，開始小慌張。後來我自創了一個 O(n * logn * logn)的解法，賣力地邊寫邊解釋了二十分鐘。考官表示我的邏輯正確，只是這不是最佳解。在我準備心灰意冷時他卻誇獎我解釋口才清晰他很欣賞，有讓我心裡舒服點。事後想想，說不定面試官本來就沒有期待 candidate 能在短時間做出這道難題，可能目的是想觀察考生在壓力下的思路而已。考完coding之後聊了簡歷，我認真‌‍‌‌‍‍‌‌‍‌‍‍‌‍‌‍‍‌‌的把本科的畢業論文渲染的厲害點，也剛好是做 RL 的，很對他胃口。面完之後我把論文附件給他郵件，希望能靠這個加點分數。

兩天後收到 HR 的郵件: The team is moving forward with an intern offer! 

這是我在美國錄取的第一份實習，當下真的很興奮。(晚上立刻去酒吧慶祝了一波，點了我最愛的 old-fashioned。)

拿到 Visa Research 的 offer 之後我不敢鬆懈，還是積極的準備之後的面試。雖然 ML Research 的職位很吸引我，但是一個月的薪資和零用金加起來只有 7K 美金左右 (不到二十萬台幣)，以美國對軟體工程師實習的市場價格來說，確實不高 (頂多達到 SWE 實習薪資的平均而已)。雖然大家都說實習的錢不是很重要，但我覺得這會影響到之後轉正職的發展，而且我偷偷相信我在市場上值得更好的待遇。


## 2. Tapad Software Engineer

![Tapad](/images/tapad.jpg){: width="33%"}

Tapad 這家規模中小型的公司，在論壇上號稱是紐約的明星新創，每年僅招 3 名實習生。最近正把辦公室搬到曼哈頓第五大道上，可見公司未來發展很被看好。


這家公司我面試了三輪 technical phone screen + BQ，工程師考官清一色都是 Carnegie Mellon University 的菁英高材生，考的題目都很有程度，而且都會有細心加深的 follow-up。第一關考題包括了資料結構和 OOP 的設計實作，一周後的第二關則是考了 Medium 難度的陣列操作。

最後一關是中高難度的演算法，我聽完題目後很快的想到最佳的時間複雜度解法，但主考官要求我把空間複雜度降成 O(1)。我思考了五分鐘之後，有個初步設計，就一邊寫一邊解釋也一邊思考接下來的步驟，最後成功通過所有 testcase。

我覺得我三輪都表現得不錯，但是一開始就知道 headcount 很少，所以心裡先做好了最壞的打算。

接到 offer call 的那瞬間我真的好開心好驕傲，感覺實力被真正肯定了，擠進一個名額很少的新創公司。

薪資方面，Tapad 一個月的 internship base salary 約為台幣二十四萬，而且地處紐約黃金地帶，我很心動。

## 3. Amazon Software Development Engineer - Bay Area, CA

![Amazon](/images/amazon animated logo.gif){: width="50%"}

因為簽了亞馬遜的 NDA，這邊不方便透漏面試細節。總之 Amazon 整體讓我感覺很順暢，很友善，面試官非常用心的聽我的每個回答，甚至在我每題 BQ 一口氣回答了五分鐘之後都會幫我用一兩句話總結，確定我想表達的意思他沒有誤會，在 coding 考題上也沒有故意為難我。感覺這次的面試重點放在我身為工程師的思路邏輯，以及是否具備解決事情的能力，而不是要我秀我刷了多少題 LeetCode。面完當下感覺很舒服，對自己表現挺滿意的。

在還沒收到 offer 之前，其實 Amazon 跟 Tapad 之間我很猶豫。因為兩家的規模跟性質很不一樣，加入新創有種跟著公司一起成長的感覺，每天都有不同的新鮮新挑戰。再者，如果 Tapad 之後 IPO，員工股票估計也會大賺一筆。

話雖如此，在 Amazon 薪水一開出來那瞬間我就幾乎確定要放棄 Tapad 了。Base salary 加上 stipend，亞馬遜實習一個月給我的總薪資逼近 13K 美金 (三十六萬台幣)，幾乎是Visa Research 的兩倍。後來聽聞消息才知道 Amazon 今年調漲薪資，一下子躍升成 FAANG 裡頭實習薪水給的最優渥的公司。除了薪水不錯以及地點在 SWE 最愛的灣區之外，履歷上多了一個大廠的頭銜，未來想挑(跳)戰(槽)別的公司應該也會容易許多。不過聽說 Amazon 內部壓力不小，work-life balance 也沒有非常友善。我要做好心理準備，畢竟天下沒有白吃的午餐。

收到 Amazon 的錄取後，我婉拒了 Visa Research 和 Tapad 的 offer (寫郵件時心如刀割)。後來還有一些投資銀行和 hedge fund 的面試邀約我也都先婉拒了，其中包含了 J.P.Morgan 還有 WorldQuant。

# 總結

這上半學年找實習的路上從一開始的焦慮，到後來的認真準備，相信船到橋頭自然直，我覺得我心態成長了不少。尤其是刷題的部分，讓我深切地感受到，只要我願意付出努力，收穫自然就會慢慢來。這邊我想感謝家人和女朋友默默給我精神上的支持，還有身邊認真的朋友當我看齊的榜樣，我才有持續的動力一直認真準備面試。對我背景有興趣的朋友可以參考[我的 LinkedIn Profile](https://www.linkedin.com/in/ianyepan/)，裡面也有我的履歷和以前的實習經驗等等。

拿到了矽谷大廠的錄取，我終於可以正大光明說自己是個 FAANG 工程師了😎。
