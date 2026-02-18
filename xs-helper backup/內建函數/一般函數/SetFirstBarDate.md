# SetFirstBarDate -  (內建函數)（一般函數）

## 語法

指定腳本執行時第一筆資料的日期(不支援交易腳本)  
SetFirstBarDate(資料開始日期)

## 說明

關於資料讀取範圍的定義，請參考[資料讀取範圍與腳本執行的關係](http://www.xq.com.tw/lesson/xspractice/%E8%B3%87%E6%96%99%E8%AE%80%E5%8F%96%E7%AF%84%E5%9C%8D%E8%88%87%E8%85%B3%E6%9C%AC%E5%9F%B7%E8%A1%8C%E7%9A%84%E9%97%9C%E4%BF%82/)。

SetFirstBarDate 函數用於控制腳本執行時，所使用的第一個資料的日期，從而確定資料讀取的起始範圍。

語法為 SetFirstBarDate(YYYYMMDD)，其中 YYYYMMDD 是起始日期的年、月、日，且必須為合理有效的日期。

需要注意的是，SetFirstBarDate **不支援交易腳本**。

**如果在腳本中多次使用 SetFirstBarDate 函數，並設定了不同的日期：**

* 將會採用其中最早（最小值）的日期作為最終的資料開始日期。

     若設定的日期不是合理有效的日期，該行的 SetFirstBarDate 將被視為編譯失敗。

**如果在腳本中同時存在數個 [SetTotalBar](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetTotalBar&group=GENERALFUNC) 和 SetFirstBarDate，並設定了不同的數值時：**

* 系統將分別根據兩者被多次使用時的規則，決定接下來各自採用哪一個 [SetTotalBar](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetTotalBar&group=GENERALFUNC) 與 SetFirstBarDate 做比較。
* 接著採用兩者當中，最後一個成功完成編譯的函數設定。

     若其中一個函數因參數無效（例如 [SetTotalBar](https://xshelp.xq.com.tw/XSHelp/?HelpName=SetTotalBar&group=GENERALFUNC) 的資料讀取筆數為負數，或 SetFirstBarDate 的日期不合理）而編譯失敗，則只有另一個成功編譯的函數設定會被採用。
