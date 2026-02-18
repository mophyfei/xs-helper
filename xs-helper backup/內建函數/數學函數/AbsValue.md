# AbsValue -  (內建函數)（數學函數）

## 語法

取絕對值。傳回無正負號的數值  
回傳數值 = AbsValue(數值)

## 說明

AbsValue函數用來計算傳入數值的**絕對值**。舉例而言：

```xs
Value1 = Abs(3);
Value2 = Abs(-3);
```

在上面範例內, Value1跟Value2的數值都是3。

以下使用AbsValue來計算兩條均線的差異，由於腳本只關心差異的大小，所以使用AbsValue函數來取得絕對值，不用考慮正負號。

```xs
Value1 = Average(Close, 5);
Value2 = Average(Close, 10);
Value3 = AbsValue(Value1 - Value2);
If Value3 <= 0.01 * Close Then Ret = 1;
```
