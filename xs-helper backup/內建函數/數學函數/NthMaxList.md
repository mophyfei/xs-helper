# NthMaxList -  (內建函數)（數學函數）

## 語法

傳入多個數值，回傳這些數值內由大到小排名第幾的數字。  
回傳數值 = NthMaxList(排名位置, 數值1, 數值2, 數值3, ..)

## 說明

**排名位置**從1開始，1表示是回傳排名第一(最大)的數字，2表示回傳排名第二(次大)的數字，以下類推。在**排名位置**之後可以傳入任意個數值，使用逗號分開。

舉例:

```xs
Value1 = NthMaxList(1, 50, 50, 40, 30);  // Value1 = 50
Value2 = NthMaxList(2, 50, 50, 40, 30);  // Value2 = 50
Value3 = NthMaxList(3, 50, 50, 40, 30);  // Value3 = 40
Value4 = NthMaxList(4, 50, 50, 40, 30);  // Value4 = 30
```

上述計算 50, 50, 40, 30 這四個數字由大到小的排名數字。請注意傳入的數值內有兩個50，分居排名1跟2。

另外一個範例:

```xs
Value1 = NthMaxList(1, Close, Close[1], Close[2], Close[3], Close[4]);
Value2 = NthMaxList(5, Close, Close[1], Close[2], Close[3], Close[4]);
```

使用NthMaxList取得近5日的最高收盤價以及最低收盤。

當排名位置為1時，NthMaxList函數等同於[MaxList函數](api?a=maxlist&b=bif)。當排名位置為最後一名時，NthMaxList函數等同於[MinList函數](api?a=minlist&b=bif)。
