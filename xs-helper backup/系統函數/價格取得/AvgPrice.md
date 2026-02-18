# AvgPrice -  (系統函數)（價格取得）

## 語法

取得利用K棒的開高低收所計算出的平均價格。  
回傳數值=AvgPrice  
---  
※請注意：AvgPrice 與 getfield("AvgPrice") 是不同的數值，  
getfield("AvgPrice") 是今日的平均成交價，也就是「當日每筆的成交金額加總／當日成交量」

## 說明

計算公式：

平均價格 = (當期開盤價 + 當期最高價 + 當期最低價 + 當期收盤價)/4

範例：

```xs
plot1(avgprice);    //繪製當天平均價格的連線
plot2(avgprice[1]); //繪製前一天平均價格的連線
```
