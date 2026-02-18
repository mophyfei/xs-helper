# Highest -  (系統函數)（價格取得）

## 語法

計算序列資料的最大值。  
回傳數值=Highest(數列,期數)  
傳入二個參數:  
- 第一個參數是數列，通常是開高低收的價格數列。  
- 第二個參數是期數。

## 說明

以最新一筆資料為基準點，輸入要計算的期數，然後計算過去期數的極大值。

Highest函數與[FastHighest](api?a=FastHighest&b=sys)函數的運算方式一致，都是用 [Extremes](api?a=Extremes&b=sys) 函數抓極大值。

範例：

```xs
plot1(Highest(high,5));    //繪製5期最高價的最大值的連線
```
