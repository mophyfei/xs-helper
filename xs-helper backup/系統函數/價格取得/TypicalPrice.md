# TypicalPrice -  (系統函數)（價格取得）

## 語法

傳回技術分析的典型價。  
回傳數值=TypicalPrice

## 說明

計算公式：

典型價 = (當期最高價 + 當期最低價 + 當期收盤價)/3

範例：

```xs
plot1(TypicalPrice);    //繪製當期典型價的連線
plot2(TypicalPrice[1]); //繪製前一期典型價的連線
```
