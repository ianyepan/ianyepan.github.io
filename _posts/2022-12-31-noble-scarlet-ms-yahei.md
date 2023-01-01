---
title: 新版微軟雅黑，以及在 Windows 上優化中文顯示的多種技巧
author: Ian Y.E. Pan
date: 2022-12-31 14:22:00 -0500
categories: [Typography]
tags: [fonts, chinese]
---

# 新版微軟雅黑，代號 Noble Scarlet

Windows 作業系統上的微軟雅黑字體是一個[劃時代的創作](https://celestialphineas.github.io/ckcers-tutorial/old-articles/microsoft-yahei/)，其原型可追溯至[方正蘭亭黑](https://www.zhihu.com/question/20425509)。近年更是擴充了繁體字形（雅黑原本只有簡體字形），也涵蓋了 GBK 中的字符，在實用度和優美程度上可說是完勝了 Windows 內建所有中文黑體，甚至包括預設繁體：“微軟正黑”。但雅黑 “中宮大”，“重心低”，“字面大” 的特點，容易在現代高畫質屏幕上顯得擁擠，不如 macOS 的[蘋方](https://zh.wikipedia.org/zh-tw/%E8%8B%B9%E6%96%B9)或是 Google/Adobe 合作設計的[思源黑體](https://zh.wikipedia.org/zh-tw/%E6%80%9D%E6%BA%90%E9%BB%91%E9%AB%94)來的優雅現代。

微軟曾在幾年前的一次 Windows Insider Preview Build（測試版）中[提到了一款新的微軟雅黑字體](https://blogs.windows.com/windows-insider/2017/10/25/announcing-windows-10-insider-preview-build-17025-pc/)，代號 "Noble Scarlet"。下圖中可見新版的雅黑 "中宮收窄"， "字距加大" 的新版設計。也有內行的網友提到，這次新版雅黑其實就是基於方正蘭亭黑 Pro，英文字符則是繼續沿用經典的 Segoe UI。

![Noble Scarlet](/images/noble-scarlet.png){: width="400"}
_新版微軟雅黑：灰色外框為原版，藍色為新版_

新版雅黑一出，好評不斷，但不知為何（有人說是因為新版雅黑在低畫質顯示屏上效果不佳），微軟並沒有在之後的 Windows 穩定版中加入這次改動。新版雅黑也從此無人問津。

好在有賢人將這曇花一現的新版雅黑打包下載，搭配第三方軟件後，可以替換系統字體，將 Windows 裏所有舊版雅黑一次換成新的 Noble Scarlet。有興趣的朋友們可以參考[此文](https://www.ugediao.com/noble-scarlet-for-windows.html)，其實就是用同名的新版雅黑強行覆蓋系統上原版的雅黑，之後再重新開機即可。效果清晰可見，如下圖。

![Old vs New Yahei](/images/old-vs-new-yahei.png){: width="400"}
_新舊微軟雅黑對照圖_

# 英文 Windows 系統底下優化中文顯示

另外，當 Windows 電腦系統預設語言是英文時，內建的軟體默認使用 Segoe UI 為系統字體。這時如果需要顯示漢文（譬如在 File Explorer 內有中文檔名），Windows 預設是用以下順序顯示字符：Meiryo, Microsoft Yahei, Microsoft Jhenghei。問題在於，Meiryo 是一個日文字體，但也包含了（較醜的）漢文字符，優先權卻高於微軟雅黑。[此文](https://shajisoft.com/shajisoft_wp/cn/%E5%AE%8C%E7%BE%8E%E8%A7%A3%E5%86%B3%E4%B8%AD%E6%96%87%E5%9C%A8%E8%8B%B1%E6%96%87windows%E4%B8%8A%E6%98%BE%E7%A4%BA%E9%AB%98%E7%9F%AE%E4%B8%8D%E4%B8%80%E7%9A%84%E9%97%AE%E9%A2%98/)提到，我們可以透過 Registry Editor, 在 `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\FontLink\SystemLink` 底下將 Segoe UI 的 “連接字體” 中微軟雅黑的地位提至 Meiryo 之上。


![Yahei Before Meiryo](/images/yahei-before-meiryo.png){: width="400"}
_優先以微軟雅黑顯示英文系統下的中文字體_


# 瀏覽器裏優化中文顯示

最後，我們在用 Chromium-based 瀏覽器（如 Google Chrome, Microsoft Edge, Brave）時會遇到簡繁字體的字形不同，造成了粗細不均的醜態。我們可以透過將瀏覽器中 Standard Font 設為 Microsoft Yahei (UI)，再搭配 Chrome 插件 "[No Per-Script Font!](https://chrome.google.com/webstore/detail/no-per-script-font/lndmkajeoopejggihiomoaepinlhblmm)"，統一使用雅黑為所有漢字的顯示字符，一併取代了較為劣質的微軟正黑體。效果如下。

![Before](/images/jhenghei-before.png){: width="400"}
_優化前_

![After](/images/yahei-after.png){: width="400"}
_優化後_

除此之外，Chrome 也在一些 UI 界面使用日語的 Yu Gothic UI 字體來顯示中文內容，例如 tab bar 及 address bar 等處。這時，最直接暴力的解決方法是再次打開 Registry Editor，在 `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts` 下 Yu Gothic 系列的 Data 稍作修改使系統認不出字體檔名（例如，將 `YuGothR.ttc` 改成 `-YuGothR.ttc`）。再前往至 `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\FontLink\SystemLink`，將 Yu Gothic UI 的對應連接字體中 “微軟雅黑” 的地位移至最高。

我也試著用同樣的方法用雅黑取代正黑，目前尚未成功（系統如果找不到正黑，只會用一個原始的宋體來顯示，忽視我設定的正黑首選連接字體：雅黑）。若之後發現可行的辦法，會再更新此文。