# AvgList -  (內建函數)（數學函數）

## 語法

傳入數個數值，回傳這些數值的的平均值。  
回傳平均值 = AvgList(數值1, 數值2, 數值3, ..)

## 說明

使用AvgList時可以傳入多個數值，數值之間使用逗號分隔，例如：

```xs
Value1 = AvgList(Open, High, Low);
```

上述範例內使用AvgList來計算Typical Price (*(開盤價 + 最高價 + 最低價) / 3*)。

請注意: 如果要計算序列型的數值的平均值的話，則可以使用[Average函數](api?a=average&b=sys)。
