# TrueRange -  (系統函數)（價格計算）

## 語法

傳回當根K棒的真實區間。  
回傳數值=TrueRange

## 說明

真實區間是韋爾達（J. W. Wilder）於1978年發表於New Concepts in Technical Trading Systems中測量價格波動性的方法。

根據韋爾達的定義，真實區間為下列三項中的最大值：

* 當期最高價至最低價的幅度。
* 當期最低價與前期收盤價的幅度。
* 當期最高價至前期收盤價的幅度。

範例：

```xs
plot1(TrueRange);    //繪製當期K棒真實區間的連線
plot2(TrueRange[1]); //繪製前一期K棒真實區間的連線
```
