# NthHighestArray -  (系統函數)（價格關係）

## 語法

計算陣列資料的第N個極大值。  
回傳數值=NthHighestArray(陣列,陣列大小,第幾個極大值)  
傳入三個參數:  
- 第一個參數是要計算的陣列。  
- 第二個參數是陣列大小。  
- 第三個參數是要計算極大值的序號。

## 說明

範例：

```xs
Array: arrA[5](0); // 宣告arrA是一個有5個元素的陣列，初始值都是0


arrA[1] = 0;  arrA[2] = 10; arrA[3] = 20; arrA[4] = 30; arrA[5] = 40;


value1 = NthHighestArray(arrA,5,3); //計算陣列arrA的第三個極大值
plot1(value1);                      //繪製陣列arrA的第三個極大值的連線
```
