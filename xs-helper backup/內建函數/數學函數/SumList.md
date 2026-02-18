# SumList -  (內建函數)（數學函數）

## 語法

傳入數個數值，回傳這些數值的的加總  
回傳加總值 = SumList(數值1, 數值2, 數值3, ..)

## 說明

使用SumList時可以傳入數個數值，數值之間用逗號隔開。

範例:

```xs
Value1 = SumList(Open, High, Low, Close) / 4;
```

上述範例內使用SumList來計算平均價格 (*(開盤價 + 最高價 + 最低價 + 收盤價) / 4*)。

請注意: 如果要計算序列型的數值的加總值的話，則可以使用[Summation函數](api?a=summation&b=sys)。
