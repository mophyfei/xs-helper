# BarsLast -  (系統函數)（日期相關）

## 語法

取得上一次條件成立到當前的K棒數  
回傳數值=BarsLast(條件數列)

## 說明

計算目前K棒與上次條件成立K棒的期數差。例如，上次KD黃金交叉是幾天前。

回傳值為0表示條件成立當期，回傳值為1表示前1期條件成立，依此類推。

範例：

```xs
value1 = average(C,5);
value2 = average(C,20);
value3 = barslast(value1 cross over value2);       //計算上次5日均線和20日均線黃金交叉的期數差
value4 = low[value3];       //取得上次均線黃金交叉時的最低價做為支撐價
plot1(value4);       //繪製支撐價的連線
```
