# SetRemoveOutlier -  (內建函數)（一般函數）

## 語法

排除Rank語法中的離群值商品，被排除的商品不會進入排行。  
SetRemoveOutlier("zscore", value:=3)  
傳入兩個參數：  
- 第一個參數為排除離群值的方式，有 "zscore" 和 "IQR"。  
- 第二個參數為排除離群值的範圍，zscore預設為3，IQR預設為1.5，此數值需大於0。

## 說明

此語法會讓離群值商品在排行前就被排除，也不會被納入計算其他屬性，例如 avgvalue。

此語法需寫在 rank 語法內，且每一個rank只能有一個。

此語法必須在rank內的最上層，不能夠放在 if 或 for 等邏輯判斷內。

以下是簡單範例：

```xs
Rank myRank Begin 
    RetVal = HVolatility(Close,20);
    SetRemoveOutlier("zscore", value:=3);
    end;
```

此範例會用波動率進行排行，但會先排除掉 zscore 絕對值大於3的商品。
