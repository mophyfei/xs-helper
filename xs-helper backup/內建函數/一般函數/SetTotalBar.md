# SetTotalBar -  (內建函數)（一般函數）

## 語法

指定腳本執行時的資料讀取範圍  
SetTotalBar(資料讀取筆數)

## 說明

關於**資料讀取範圍**的定義，請參考[資料讀取範圍與腳本執行的關係](http://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

SetTotalBar 用於設定腳本執行時，讀取的歷史K棒數量。

SetTotalBar 設定的資料讀取筆數必須為**非負整數**。當腳本引用 SetTotalBar 並指定一個數值時，系統會預先提供指定數值的K棒，並從這些K棒的第一根 (編號為1) 開始執行腳本。

不同腳本類型的規則：

* 指標腳本： 將繪製的歷史 K 棒數量，不包含即時的 K 棒。
* 選股腳本： 判斷目前K棒是否滿足選股條件之前，將執行的 K 棒數量。
* 警示腳本： 開始接收即時價格更新並觸發警示訊號之前，將執行的 K 棒數量，不包含即時的 K 棒。
* 自動交易腳本：([策略部位計算起點](https://www.xq.com.tw/learn/xsat/parameters/#learn04)之前，或)開始接收即時價格更新並觸發警示訊號之前，將執行的 K 棒數量，不包含即時的 K 棒。

**如果在腳本中多次使用 SetTotalBar ，並設定了不同的數值：**

* 將會採用其中編譯成功的最大數值作為最終的K棒總數。

     若設定的數值不是非負整數，該行的SetTotalBar將被視為編譯失敗。

**如果在腳本中同時存在數個 SetTotalBar 和 [SetFirstBarDate](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetFirstBarDate&group=GENERALFUNC) ，並設定了不同的數值時：**

* 系統將分別根據兩者被多次使用時的規則，決定接下來各自採用哪一個 SetTotalBar 與 [SetFirstBarDate](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetFirstBarDate&group=GENERALFUNC) 做比較。
* 接著採用兩者當中，最後一個成功完成編譯的函數設定。

     若其中一個函數因參數無效（例如 SetTotalBar 的資料讀取筆數為負數，或 [SetFirstBarDate](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetFirstBarDate&group=GENERALFUNC) 的日期不合理）而編譯失敗，則只有另一個成功編譯的函數設定會被採用。
