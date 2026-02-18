# xf_XAverage -  (系統函數)（跨頻率）

## 語法

計算指定頻率的指數移動平均。  
回傳數值=xf\_XAverage(頻率,數列,期數)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列，通常是開高低收的價格數列。  
- 第三個參數是期數。

## 說明

xf\_XAverage是[XAverage](api?a=XAverage&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的XAverage值。

範例：

```xs
value1 = xf_XAverage("W",GetField("Close","W"),5); //計算週線5期收盤價的指數移動平均
```
