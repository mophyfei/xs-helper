# TrueHigh -  (系統函數)（價格取得）

## 語法

取得價格真實區間(TrueRange)的高點。  
回傳數值=TrueHigh

## 說明

計算方法為比較當根K棒的高點與前根K棒的收盤價，取數值較大者。

範例：

```xs
plot1(TrueHigh);    //繪製當期真實區間高點的連線
plot2(TrueHigh[1]); //繪製前一期真實區間高點的連線
```

請參考 [TrueLow函數](api?a=truelow&b=sys)以及[TrueRange函數](api?a=truerange&b=sys)。
