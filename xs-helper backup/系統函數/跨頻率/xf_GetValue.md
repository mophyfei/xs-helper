# xf_GetValue -  (系統函數)（跨頻率）

## 語法

引用指定頻率的數值。  
回傳數值=xf\_GetValue(頻率,數列,期別)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列。  
- 第三個參數是期別，相對目前而言要往前的筆數。

## 說明

在同一個頻率時，我們可以直接利用\*\*變數[3]\*\*取得前3期的變數值。當資料頻率不同時（跨頻率），我們就需要使用xf\_GetValue或xf\_GetBoolean來取得之前的變數值。若變數是數值時，要用xf\_GetValue；若變數是布林值時，要用xf\_GetBoolean。

```xs
value1 = xf_WeightedClose("W");            //計算週線的加權平均價
value2 = xf_GetValue("W",value1,1);        //取得上一週的加權平均價
plot1(value2);
plot2(value1[1]);                        //可以比較一下和value2的差異
```

相關函數：[xf\_GetBoolean](api?a=xf_GetBoolean&b=sys)。
