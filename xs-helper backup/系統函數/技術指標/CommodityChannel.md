# CommodityChannel -  (系統函數)（技術指標）

## 語法

計算CCI技術指標值。  
回傳數值=CommodityChannel(期數)

## 說明

CCI順勢指標為Donald Lambert在1980年代發明的指標。CCI大多時間會落在+100到-100之間，主要的功能在於針對商品本身所具有的週期循環特性，來尋找出行情的高低價位區來。

若CCI值從一高點急速往下降時，可視為是賣出的訊號；同理若CCI值從一低點快速往上升時，可視之為買進的訊號。

範例：

```xs
value1 = CommodityChannel(14);       //計算14期的CCI
plot1(value1, "CCI");
```

註：CommodityChannel函數和[CCI](api?a=CCI&b=sys)函數相同。
