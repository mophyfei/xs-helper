# CountIfARow -  (系統函數)（邏輯判斷）

## 語法

計算符合條件連續符合的次數。  
回傳數值=CountIfARow(條件數列,期數)  
傳入二個參數:  
- 第一個參數是條件數列。  
- 第二個參數是期數。

## 說明

以最新的資料為基準，往前計算連續符合條件的次數。

範例：

```xs
value1 = CountIfARow(close>close[1],5); //計算過去5天連續收紅的次數
```

註：CountIfARow函數和[TrueCount](api?a=TrueCount&b=sys)函數相同。
