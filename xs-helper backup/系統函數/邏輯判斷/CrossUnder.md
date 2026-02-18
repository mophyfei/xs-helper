# CrossUnder -  (系統函數)（邏輯判斷）

## 語法

判斷數列一是否由上往下穿越數列二，又稱死亡交叉。  
回傳布林值=CrossUnder(數列一,數列二)  
傳入二個參數:  
- 第一個參數是數列一。  
- 第二個參數是數列二。

## 說明

如果出現死亡交叉傳回True，其他狀況傳回False。

範例：

```xs
condition1 = CrossUnder(Average(close,5),Average(close,10)); //判斷5期均線和10期均線是否死亡交叉
```
