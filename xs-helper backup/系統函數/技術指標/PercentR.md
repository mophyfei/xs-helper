# PercentR -  (系統函數)（技術指標）

## 語法

計算威廉指標（或稱百分比R、%R）。  
回傳數值=PercentR(期數)

## 說明

由Larry Williams所提出，將當前收盤價在指定期間內的區間最高價與最低價之間位置，以百分比表示。

範例：

```xs
value1 = PercentR(12) - 100;       //計算12期的威廉指標
plot1(value1, "%R");
```
