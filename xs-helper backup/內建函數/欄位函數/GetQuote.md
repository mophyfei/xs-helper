# GetQuote -  (內建函數)（欄位函數）

## 語法

讀取系統內的報價欄位資料 (僅支援警示、交易腳本)  
報價欄位數值 = GetQuote(欄位名稱)  
報價欄位數值 = q\_欄位英文名稱  
報價欄位不支援回測功能

## 說明

在XS腳本內除了可以讀取資料欄位之外([GetField函數](api?a=getfield&b=bif))，也可以讀取即時的報價欄位。

即時報價欄位涵蓋的範圍有:

* 由交易所提供的即時報價，如五檔委託價量以及即時成交等資料
* 由系統統計的市場行情資訊，如加權指數漲跌家數，當日內外盤量等資料
* 商品的最新一期基本資料相關數據，例如發行股數，每股淨值，最新一季的毛利率等

使用者可以點選XS編輯器內的編輯選單/插入欄位選項，或是按快捷鍵F7，在插入欄位畫面內點選**報價**標籤，然後就可以搜尋或是瀏覽需要的報價欄位。

使用者可以在XS編輯器中的功能列按下「欄位」圖示，或是按快捷鍵F7，進入插入欄位視窗後，先選取「報價欄位」標籤頁，再點選欲加入報價欄位的名稱，最後按下「插入」按鈕，就會在XS編輯器中產生 q\_ 開頭的 GetQuote 報價欄位語法。

![插入欄位GetQ](https://www.xq.com.tw/wp-content/uploads/2021/07/0722Y21_GetQ.png)

目前即時報價欄位僅能使用在警示與交易類型的腳本。而且由於報價欄位的數值是採用即時更新的方式，只提供最新的數值，無法取得前期值，因此無法使用在回測上，在使用上要注意這個特性。

以下是一個使用即時報價欄位的範例:

```xs
Input: OpenGap(1);
Input: Downpercent(1);


if  q_DailyHigh = q_DailyOpen and
    q_DailyOpen > q_RefPrice * (1+ OpenGap/100) and
    q_Last < q_DailyHigh * (1 - Downpercent/100)
then ret=1;
```

上面的這個警示腳本內判斷如果當日開在最高([q\_DailyHigh](api?a=q_dailyhigh) = [q\_DailyOpen](api?a=q_dailyopen))，而且是跳空開出 (q\_DailyOpen > [q\_RefPrice](api?a=q_refprice) \* (1 + 跳空比例/100)，而目前的價格 [q\_Last](api?a=q_last)已經回落一定範圍的話則觸發警示。在這裡q\_DailyHigh為當日的最高價, q\_DailyOpen為當日的開盤價, q\_RefPrice為當日的參考價, q\_Last為當日的最新價格，這些都是常用的報價欄位。
