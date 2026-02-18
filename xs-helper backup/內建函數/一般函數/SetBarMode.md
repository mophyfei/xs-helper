# SetBarMode -  (內建函數)（一般函數）

## 語法

指定腳本執行時的函數計算方式  
SetBarMode(函數計算方式)

## 說明

SetBarMode可以指定函數的計算方式，分別為(0),(1),(2)

對於Setbarmode 0,1,2 三種計算方式解釋:

```xs
SetBarMode(0); //Auto，預設值
```

由系統判定是simple函數 或是 series 函數

```xs
SetBarMode(1);  //指定為simple函數
```

Simple型態是指，例如average 這類函數計算方式，今期所計算的平均數與前一期的平均數為個別獨立運用，不會相互有關係
例如:平均數average
(1,2,3,4,5)/5 = 3 ；(2,3,4,5,6)/5 = 4
兩者計算的結果無關聯

```xs
SetBarMode(2);  //指定為series函數
```

Series 型態是指，例如MACD,RSI 指標，屬於連續性的數值，今期所計算的值會引用到前期的數值來做運算。

例如RSI指標計算「期間內絕對漲幅」的公式為
UP t = UP t-1 + 1 / N ( Ut – UP t-1) ，
(N 為平滑平均天數， t 為當日值， t-1為前一日值)
當期值t會使用到前期的(t-1) 進行運算
