# Second -  (內建函數)（時間函數）

## 語法

計算傳入時間的秒數  
秒數 = Second(時間數值)  
回傳秒數的數值範圍從0到59

## 說明

時間數值是一個5~6碼的數字，格式是HHMMSS:

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字

時間數值通常是透過CurrentTime，Time(資料的時間欄位)，或是其他時間相關函數所產生的時間數值。

```xs
Value1 = CurrentTime;
If Hour(Value1) = 13 And Minute(Value1) = 05 And Second(Value1) >= 30 Then
Begin
	// 目前時間 >= 13:05:30
End;
```

---

時間數值也可以是一個含有毫秒的 8~9 碼的數字，格式是HHMMSS.fff：

* HH是小時，數值範圍從0到23，可以是1位數字或是2位數字
* MM是分鐘，數值範圍從00到59，必須是2位數字
* SS是秒數，數值範圍從00到59，必須是2位數字
* fff是毫秒，數值範圍從000到999，必須是3位數字

含有毫秒的時間數值，通常是透過CurrentTimeMS，[FilledRecordTimeMS](api?a=FilledRecordTimeMS&b=bif)，或是其他時間相關函數所產生的時間數值。

以下的範例是用 CurrentTimeMS 傳入的時間數值

```xs
Value2 = CurrentTimeMS;
If Hour(Value2) = 13 And Minute(Value2) = 05 And Second(Value2) >= 30 Then
Begin
	// 目前時間 >= 13:05:30
End;
```

時間相關的的函數請參考[Hour函數](api?a=hour&b=bif)，[Minute函數](api?a=minute&b=bif)，[Second函數](api?a=second&b=bif)，以及[MilliSecond](api?a=millisecond&b=bif)
