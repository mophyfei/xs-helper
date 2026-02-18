# xf_CrossUnder -  (系統函數)（跨頻率）

## 語法

判斷指定頻率的數列一是否由上往下穿越數列二，又稱死亡交叉。  
回傳布林值=xf\_CrossUnder(頻率,數列一,數列二)  
傳入三個參數:  
- 第一個參數是頻率，指定傳入數列的資料期別，支援"D", "W", "M", "AD", "AW", "AM"。  
- 第二個參數是目標頻率的數列一。  
- 第三個參數是目標頻率的數列二。

## 說明

如果出現死亡交叉傳回True，其他狀況傳回False。

範例：

```xs
condition1 = xf_CrossUnder("W",Average(GetField("close","W"),5),Average(GetField("close","W") ,10) ); //判斷週線5期均線和10期均線是否死亡交叉
```
