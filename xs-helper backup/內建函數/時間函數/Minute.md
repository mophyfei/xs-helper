# Minute -  (內建函數)（時間函數）

## 語法

計算傳入時間的秒數  
分鐘數 = Minute(時間數值)  
回傳分鐘數的數值範圍從0到59

## 說明

時間數值是一個5~6碼的數字，格式是HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

```xs
Value1 = Minute(Time);
If Value1 = 0 Then
Begin
	// 目前分鐘K棒資料時間是0分
End;
```

上述範例取得目前分鐘K棒資料時間的分鐘數值。

---

時間數值也可以是一個8~9碼，含有毫秒的數字，格式是HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

含有毫秒的時間數值，通常是透過CurrentTimeMS，[FilledRecordTimeMS](api?a=FilledRecordTimeMS&b=bif)，或是其他時間相關函數所產生的時間數值。

```xs
Value1 = Minute(CurrentTimeMS);
If Value1 = 0 Then
Begin
	// 目前分鐘K棒資料時間是0分
End;
```

上述範例，也是取得目前分鐘K棒資料時間的分鐘數值，不過是傳入含有毫秒的目前電腦時間。

時間相關的的函數請參考[Hour函數](api?a=hour&b=bif)，[Minute函數](api?a=minute&b=bif)，以及[Second函數](api?a=second&b=bif)。
