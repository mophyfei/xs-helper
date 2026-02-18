# Array_SetMaxIndex -  (內建函數)（陣列函數）

## 語法

重新設定陣列的大小  
僅支援一維陣列  
Array\_SetMaxIndex(陣列，陣列內的元素個數)

## 說明

設定[動態陣列](api?a=array&b=keyword)的大小。

```xs
Var: Count(0);
Array: NumArray[](0);


If High > Highest(High,20)[1] Then Count = Count + 1;


Array_SetMaxIndex(NumArray, Count);
NumArray[Count] = High;
```

在上述範例內，我們希望可以儲存破20期新高的所有價格。由於執行過程內可能會發生多次創新高的情形，所以我們使用陣列來儲存這些創新高的價位。又由於無法知道創新高的出現次數，所以程式使用動態陣列來儲存這些價格。在上面的範例內，Count就是目前已經創新高的個數，而當又出現創新高的情形時，程式就使用Array\_SetMaxIndex來擴充陣列的大小。
