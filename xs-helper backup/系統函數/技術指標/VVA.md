# VVA -  (系統函數)（技術指標）

## 語法

計算VVA指標。  
回傳數值=VVA

## 說明

OBV的變形累算方法，除了VA成交量累散佈指標以外，還有另外一種的變化方式，即VVA指標(Variable Volume Accumulation/Distribution)。

VVA屬於累積型的指標，指標值會因計算的起始點不同而有所差異。

範例：

```xs
value1 = VVA;       //計算VVA指標
plot1(value1, "VVA");
```
