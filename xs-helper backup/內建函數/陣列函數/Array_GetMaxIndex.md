# Array_GetMaxIndex -  (內建函數)（陣列函數）

## 語法

取得陣列內的元素個數  
元素個數 = Array\_GetMaxIndex(陣列變數)

## 說明

回傳陣列內的元素個數。

```xs
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


Value1 = Array_GetMaxIndex(arrA);  // Value1 = 5
```

我們可以利用這個函數來動態取得陣列的大小，讓程式更容易維護：

```xs
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0
Var: I(0);


For I = 1 To Array_GetMaxIndex(arrA) 
Begin
  arrA[I] = arrA[I] + 1;
End;
```

在上述範例內雖然我們已經知道arrA的大小為5，可是我們還是可以利用 Array\_GetMaxIndex 來取得 arrA 的大小。未來程式如果有需要調整arrA的大小時，程式內迴圈的程式碼可以不需要修改，方便程式的維護。
