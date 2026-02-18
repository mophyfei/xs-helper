# ADO -  (系統函數)（技術指標）

## 語法

計算聚散擺盪指標值。  
回傳數值=ADO

## 說明

ADO指標的原文是Accumulation／Distribution Oscillator，直譯為「聚散擺盪」指標。

計算公式如下：

ADO＝( BP + SP ) ／ ( 2 \* ( 最高價 － 最低價 ) ) \* 100

其中

* BP = 最高價－開盤價
* SP = 收盤盤－最低價

範例：

```xs
value1 = ADO;              //計算聚散擺盪指標
plot1(value1, "A/D-Osc");
```
