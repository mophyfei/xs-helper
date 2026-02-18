# xf_PercentR -  (系統函數)（跨頻率）

## 語法

計算指定頻率的威廉指標值。  
回傳數值=xf\_PercentR(頻率,期數)  
傳入二個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是計算威廉指標的期數。

## 說明

xf\_PercentR是[PercentR](api?a=PercentR&b=sys) 函數的跨頻率版本，增加了指定頻率的參數，可以計算指定頻率的PercentR值。

範例：

```xs
value1 = xf_PercentR("W", 14) - 100;       //計算週線威廉指標
Plot1(value1, "週威廉指標");
```
