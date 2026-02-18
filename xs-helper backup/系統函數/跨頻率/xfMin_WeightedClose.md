# xfMin_WeightedClose -  (系統函數)（跨頻率）

## 語法

計算指定頻率的加權平均收盤價。  
回傳數值=xfMin\_WeightedClose(頻率)  
傳入一個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"1","5","10","15","30","60","D", "W", "M", "AD", "AW", "AM"。  
備註：商品類型僅支援台股與台期權。不支援XS選股、XS選股自訂排行與XS選股回測。

## 說明

xfMin\_WeightedClose是[xf\_WeightedClose](api?a=xf_WeightedClose&b=sys) 函數的跨頻率加強版本，增加了指定分鐘頻率的參數，可以計算指定分鐘頻率的WeightedClose值。

範例：

```xs
plot1(xfMin_WeightedClose("30"));    //繪製30分鐘線加權平均收盤價的連線
```
