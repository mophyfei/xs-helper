# Array_Sort2d -  (內建函數)（陣列函數）

## 語法

把二維陣列內的某段元素進行排序。  
Array\_Sort2d(陣列，執行排序的開始位置，執行排序的結束位置，排序的比較欄位，排序的順序)  
排序的順序如果是true的話則由小排到大，如果是false的話則由大排到小

## 說明

Array\_Sort2d需要傳入五個參數:

* 第一個參數是要排序的二維陣列變數，
* 第二個參數是這個陣列內執行排序的開始位置。位置從1開始，
* 第三個參數是這個陣列內執行排序的結束位置。位置從1開始，
* 第四個參數是決定排序的基準位置。位置從1開始，
* 第五個參數決定排序的順序，如果是**true**的話，則由小排到大，如果是**false**的話，則由大排到小，

執行後這個陣列內指定範圍內元素將會依照指定的排序方式重新排列。

舉例:

```xs
Array: datum[15, 6](0); // 宣告datum是一個有15（列）6（行）的二維陣列，初始值都是0
var:i(0);


for i = 1 to 15 begin
   datum[i, 1] = time[i];
   datum[i, 2] = open[i];
   datum[i, 3] = high[i];
   datum[i, 4] = low[i];
   datum[i, 5] = close[i];
   datum[i, 6] = volume[i];
end;
 
array_sort2d(datum, 1, 15, 6, true); //datum = [最小volume, 次小volume, ... 最大volume]
array_sort2d(datum, 1, 15, 6, false); //datum = [最大volume, 次大volume, ... 最小volume]
```

上例內第一次呼叫array\_sort2d時，傳入的順序是true，所以會從小排到大，執行後datum的內容以第六行排序，排序後同列的資料會以第六行為基準一起移動。

第二次呼叫array\_sort2d時，傳入的順序是false，所以會從大排到小，執行後datum的內容以第六行排序，排序後同列的資料會以第六行為基準一起移動。
