# Array_GetType -  (內建函數)（陣列函數）

## 語法

回傳陣列的資料類型  
資料類型 = Array\_GetType(陣列)  
回傳數值如果是2，表示為邏輯值True/False陣列，如果是3，表示為字串陣列，如果是7，則表示為數值陣列

## 說明

請看下列範例程式跟註解說明:

```xs
Array: arrNumber[5](0);
Array: arrString[5]("");
Array: arrBoolean[5](true);


Value1 = Array_GetType(arrNumber);  // Value1 = 7
Value2 = Array_GetType(arrString);     // Value2 = 3
Value3 = Array_GetType(arrBoolean); // Value3 = 2
```
