# ArrayXDaySeries -  (系統函數)（Array函數）

## 語法

以陣列儲存跨頻率的序列值。  
回傳陣列=ArrayXDaySeries(序列,最大引用筆數,陣列)  
傳入三個參數:  
- 第一個參數是序列。  
- 第二個參數是最大引用筆數。  
- 第三個參數是陣列。

## 說明

以Array儲存跨頻率的序列值，傳入一個序列。

範例：

```xs
Array: CloseArray[](0);
ArrayXDaySeries(GetField("收盤價","D"),SBB_length,_DayValue);
```
