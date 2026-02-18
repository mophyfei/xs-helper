# LowestArray -  (系統函數)（價格關係）

## 語法

計算陣列資料的最小值。  
回傳數值=LowestArray(陣列,陣列大小)  
傳入二個參數:  
- 第一個參數是要計算的陣列。  
- 第二個參數是陣列大小。

## 說明

範例：

```xs
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arrA[1] = 0;  arrA[2] = 10; arrA[3] = 20; arrA[4] = 30; arrA[5] = 40;


value1 = LowestArray(arrA,5);  //計算陣列arrA的極小值
plot1(value1);                 //繪製陣列arrA的極小值的連線
```
