# GetTBMode -  (內建函數)（一般函數）

## 語法

取得自定指標的繪圖模式  
回傳數值= GetTBMode

## 說明

取得自定指標的繪圖模式。
回傳數值如下

* SetTBMode(1) 同v5.62行為，腳本資料計算筆數為資料讀取筆數加畫面上的K棒筆數，使用者拉動畫面會進行重算。
* SetTBMode(0) 預設值，腳本資料計算筆數為全部資料，整個數列只算一次，拉動畫面不會重算。

範例:

```xs
Input: Period(200, "EMA");
Input: TB(1);
SetTBMode(TB);//指定自定指標的繪圖模式，可以變更參數比較一下計算值的差異
Plot1(EMA(Close, Period), "EMA");
Plot2(GetTBMode);//取得自定指標的繪圖模式
```
